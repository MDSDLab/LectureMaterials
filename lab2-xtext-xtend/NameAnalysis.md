# Névelemzés (name analysis, scoping, validation)

A névelemzés feladata, hogy összekösse a nevek felhasználását a nevek definíciójával (pl. változók esetén).

Az Xtext automatikusan képes bizonyos szintű névfeloldást megvalósítani a szögletes zárójelek (`[...]`) segítségével megadott kereszthivatkozásokon keresztül. Egyes esetekben azonban az Xtext algoritmusa nem elég okos. Ilyenkor nekünk kell segítséget adni az Xtext számára. Erre szolgál a scoping, amelyről részletes leírás [itt található](https://eclipse.dev/Xtext/documentation/303_runtime_concepts.html#scoping).

Ezen túlmenően hibaüzeneteket kell generálnunk akkor, ha valami probléma adódik a nevek definíciójával vagy a nevek feloldásával. A feloldással kapcsolatos problémákat az Xtext automatikusan jelzi. A definíciókkal (pl. duplikált nevek) kapcsolatos hibákat nekünk kell jeleznünk. Ezt a validáció során tehetjük meg, amelyről részletes leírás [itt található](https://eclipse.dev/Xtext/documentation/303_runtime_concepts.html#validation).

## Scoping

Az Xtext megpróbálja a neveket a kód hierarchiának megfelelő módon feloldani, azonban lehetnek olyan helyzetek, amikor a hierarchia mentén történő feloldás nem elég. A WebTest nyelvben ilyen helyzet a `context as [Page] ... end` konstrukció belseje, ahol a hivatkozott oldalban definiált változókat és műveleteket fel kell tudni oldani a kontextuson belül.

A **webtest.dsl** projekten belül az **src** könyvtár alatt a **webtest.dsl.scoping** csomagból nyissátok meg a **WebTestDslScopeProvider.xtend** fájlt, és módosítsátok a tartalmát az alábbi módon:

```
package webtest.dsl.scoping

import org.eclipse.emf.ecore.EObject
import org.eclipse.emf.ecore.EReference
import org.eclipse.xtext.EcoreUtil2
import org.eclipse.xtext.scoping.Scopes
import webtest.model.ContextStatement
import webtest.model.ModelPackage

/**
 * This class contains custom scoping description.
 * 
 * See https://www.eclipse.org/Xtext/documentation/303_runtime_concepts.html#scoping
 * on how and when to use it.
 */
class WebTestDslScopeProvider extends AbstractWebTestDslScopeProvider {

    override getScope(EObject obj, EReference ref) {
        var baseScope = super.getScope(obj, ref)
        if (ref == ModelPackage.Literals.VARIABLE_EXPRESSION__VARIABLE) {
            var context = EcoreUtil2.getContainerOfType(obj, typeof(ContextStatement))
            if (context?.page !== null) {
                return Scopes.scopeFor(context.page.variables, baseScope) 
            } 
        }
        return baseScope;
    }
    
}
```

A fenti kód a változó hivatkozásoknál (**VariableExpression** osztály **variable** property-je: **ModelPackage.Literals.VARIABLE_EXPRESSION__VARIABLE**) megkeresi a hivatkozást közvetlenül tartalmazó `context as` kontextust (**ContextStatement**) az **EcoreUtil2.getContainerOfType** segédfüggvénnyel. Ha van ilyen kontextus és létezik hozzá weboldalmodell (**page**), akkor a hivatkozott weboldalmodell változóit elérhetővé teszi névfeloldásra, illetve ha a feloldás ezek közül a változók közül nem sikerül, akkor továbbkeres a **baseScope**-ban, ami az Xtext alapértelmezett hierarchikus névfeloldásából jön.

A laborfeladat során ezt a **WebTestDslScopeProvider** osztályt kell továbbfejlesztenetek:

* Tegyétek elérhetővé a `context as` kontextuson belül a változókat, de ne csak a közvetlenül tartalmazó kontextusból, hanem az összes indirekten tartalmazó kontextusból is! A mélyebben lévő kontextusok változói elfedik a kijjebb lévő kontextusok azonos nevű változóit.
* Tegyétek elérhetővé a `context as` kontextuson belül az operációkat is, de ne csak a közvetlenül tartalmazó kontextusból, hanem az összes indirekten tartalmazó kontextusból is! A mélyebben lévő kontextusok operációi elfedik a kijjebb lévő kontextusok azonos nevű operációit.
* Ügyeljetek arra, hogy az utasításoknál csak a korábban definiált változókra szabad hivatkozni, egy később definiált változóra nem!
* Ha a **BasePages** bővítményt meg kell valósítanotok, tegyétek elérhetővé a hivatkozott oldal őseiben szereplő változókat és operációkat is! Ügyeljetek arra, hogy az esetleges körkörös öröklődés ne okozzon végtelen ciklust!
* Ha a **ForEach** bővítményt meg kell valósítanotok, ügyeljetek arra, hogy a `foreach ... in ... end` fejlécében definiált változó csak az adott `foreach` blokkon belül érhető el! Arra is figyeljetek, hogy két egymás utáni `foreach` blokk definiálhatja ugyanazt a nevű változót, és ez a két változó nem ugyanaz!
* Ha a **TestParams** bővítményt meg kell valósítanotok, tegyétek elérhetővé a teszten belül a tesztparaméterek neveit!

Figyeljetek arra is, hogy szerkesztés közben nem mindig teljes a modell: előfordulhatnak null értékek olyen helyeken is, ahol nem számítunk rá.

***TIPP:** A **getScope** függvényben többféle stratégiát is követhettek:*

* *Elhagyhatjátok a **super** scope-ra vonatkozó hívást, és az összes tartalmazó objektum bejárásával feloldhatjátok az összes lehetséges referenciát egy menetben.*
* *Meghívhatjátok a **getScope** függvényt rekurzívan a tartalmazó objektumra, így jobban szeparálni lehet a felelősségeket.*
* *Tetszőleges segédfüggvényeket használhattok.*

*A modell bejárásában sok segítséget nyújthatnak az alábbi konstrukciók:*

* *Az **EcoreUtil2** osztály **getContainerOfType** függvénye megadja az adott modellbeli objektum legközelebbi olyan tartalmazó objektumát, amely az általunk kívánt típusnak megfelel. Ha az objektum maga már a kívánt típusú, akkor magával az objektummal tér vissza.*
* *Az **EcoreUtil2** osztály **getAllContainers** függvénye megadja az adott modellbeli objektum összes direkt és indirekt tartalmazó objektumát.*
* *Az **EcoreUtil2** osztály **getAllContentsOfType** függvénye megadja az adott modellbeli objektum által tartalmazott összes olyan objektumot, amely az általunk kívánt típusnak megfelel.*
* *Az **instanceof** kulcsszó használata is hasznos lehet.*

## Validation

Ha egy nevet nem sikerült feloldani, azt az Xtext automatikusan hibaüzenetként jelzi. A duplikált neveket azonban nekünk kell jeleznünk. Ezt validációk segítségével tehetjük meg.

A **webtest.dsl** projekten belül az **src** könyvtár alatt a **webtest.dsl.validation** csomagban hozzatok létre egy **NameValidator.xtend** fájlt, a következő tartalommal:

```
package webtest.dsl.validation

import java.util.HashSet
import org.eclipse.xtext.validation.Check
import org.eclipse.xtext.validation.EValidatorRegistrar
import webtest.model.Main
import webtest.model.ModelPackage
import webtest.model.Page

class NameValidator extends AbstractWebTestDslValidator {
    // This empty override is necessary to prevent reporting duplicate validation messages
    override register(EValidatorRegistrar registrar) {
    }
    
    @Check(NORMAL)
    def checkDuplicatePageNames(Main main) {
        try {
            val names = new HashSet<String>()
            for (p: main.declarations.filter(Page)) {
                if (!names.add(p.name)) {
                    error("Page '"+p.name+"' is already defined.", p, ModelPackage.Literals.NAMED_ELEMENT__NAME)
                }
            }
        } catch (Exception ex) {
            ex.printStackTrace
        }
    }
}
```

Ez a **checkDuplicatePageNames** függvényben ellenőrzi, hogy minden **Page** neve egyedi-e. Amennyiben nem, hibaüzenetet készít az ismétlődő nevekhez: `Page '<name>' is already defined.`

A hibaüzenet az oldal nevéhez kötve jelenik meg: mivel a **Page** a **NamedElemenet**-ből származik, az oldal nevét a **NamedElement** osztály **name** property-je reprezentálja.

A **@Check** annotációval Jelezzétek, hogy a **checkDuplicatePageNames** egy olyan függvény, amelyet a kód validációja során meg kell hívni. A **NORMAL** paraméter azt jelzi, hogy a kód mentésekor ellenőrizzük a feltételeket. A **checkDuplicatePageNames** függvény paramétere az az objektum, amelyen az ellenőrzést le kell futtani. Jelen esetben a teljes modell gyökerén (**Main**) keresztül bejárjuk az összes **Page** objektumot, de más validációk esetén másfajta objektum is megadható paraméterként, ilyenkor az összes szóba jöhető objektumra meghívódik a validáló függvény.

Még egy lépést el kell végeznünk, hogy a fenti validáció lefusson. A **webtest.dsl** projekten belül az **src** könyvtár alatt a **webtest.dsl.scoping** csomagból nyissátok meg a **WebTestDslValidator.xtend** fájlt, és módosítsátok a tartalmát az alábbi módon:

```
package webtest.dsl.validation

import org.eclipse.xtext.validation.ComposedChecks

/**
 * This class contains custom validation rules. 
 *
 * See https://www.eclipse.org/Xtext/documentation/303_runtime_concepts.html#validation
 */
@ComposedChecks(validators=#[NameValidator])
class WebTestDslValidator extends AbstractWebTestDslValidator {

}
```

Az Xtext ugyanis csak a **WebTestDslValidator** validátort regisztrálja be automatikusan. Ennek a **@ComposedChecks** annotációval megadhatjuk, mely más validátorokat vegyen még figyelembe. A későbbiekben még további validátorokat is fogtok majd ide regisztrálni.

A laborfeladat során fejlesszétek tovább a **NameValidator** osztályt:

* Jelezzétek, ha a tesztek nevei duplikálva vannak: `Test case '<name>' is already defined.`
* Jelezzétek, ha a változók nevei duplikálva vannak: `Variable '<name>' is already defined.`
* Jelezzétek, ha az operációk nevei duplikálva vannak: `Operation '<name>' is already defined.`
* Jelezzétek, ha egy operáció paraméterei duplikálva vannak: `Parameter '<name>' is already defined.`
* Jelezzétek, ha egy operáció lokális változója elfedi az operáció valamelyik paraméterét: `Variable '<name>' is already defined.`
* Jelezzétek, ha egy operáció meghívásakor az argumentumok száma különbözik a paraméterek számától: `Operation '<name>' has <parameter count> parameters but <argument count> arguments were specified.`
* Jelezzétek, ha egy operáció meghívásakor a névvel rendelkező argumentumok duplikálva vannak: `Argument '<name>' is already defined.`
* Jelezzétek, ha egy operáció meghívásakor egy névvel rendelkező argumentum neve nem egyezik meg egyetlen paraméter nevével sem: `Operation '<operation name>' does not have a parameter named '<parameter name>'.`
* Jelezzétek, ha egy operáció névvel rendelkező argumentumokkal történő meghívásakor valamelyik paraméter nem kap értéket: `Argument for parameter '<name>' missing.`
* Ha a **BasePages** bővítményt meg kell valósítanotok:
  * Jelezzétek, ha körkörös öröklődés van: `Page '<name>' is in a circular inheritance.`
  * Jelezzétek, ha az ősökben definiált változók ütköznek, és ezért a leszármazottnak explicit felül kell definiálnia a változót: `Variable '<variable name>' is inherited ambiguously, it must be overriden in page '<page name>'.`
  * Jelezzétek, ha az ősökben definiált operációk ütköznek, és ezért a leszármazottnak explicit felül kell definiálnia az operációt: `Operation '<operation name>' is inherited ambiguously, it must be overriden in page '<page name>'.`
* Ha a **ForEach** bővítményt meg kell valósítanotok, Jelezzétek, ha a `foreach` fejlécében definiált változó ütközik egy másik lokális változóval: `Variable '<name>' is already defined.`
* Ha a **Manual** bővítményt meg kell valósítanotok, Jelezzétek, ha a manual-ok nevei duplikálva vannak: `Manual '<name>' is already defined.`
* Ha a **TestParams** bővítményt meg kell valósítanotok:

  * Jelezzétek, ha egy tesztesetben a tesztparaméterek nevei duplikálva vannak: `Parameter '<name>' is already defined.`
  * Jelezzétek, ha egy lokális változó elfedi egy teszt valamelyik paraméterét: `Variable '<name>' is already defined.`
  * Jelezzétek, ha egy teszteset egy névvel rendelkező argumentumának neve nem egyezik meg egyetlen tesztparaméter nevével sem: `Test case '<test case name>' does not have a parameter named '<parameter name>'.`
  * Jelezzétek, ha egy tesztesetben a névvel rendelkező argumentumok a `with` kulcsszó után duplikálva vannak: `Argument '<name>' is already defined.`
  * Jelezzétek, ha egy teszteset névvel rendelkező argumentumokkal történő konfigurálásakor a `with` kulcsszó után valamelyik paraméter nem kap értéket: `Argument for parameter '<name>' missing.`
  * Jelezzétek, ha egy teszteset névvel rendelkező argumentumokkal történő konfigurálásakor a `with` kulcsszó után az argumentumok száma különbözik a paraméterek számától: `Test case '<name>' has <parameter count> parameters but <argument count> arguments were specified.`

***TIPP:** A validációk lehetőleg minél gyorsabbak legyenek, bonyolult és hosszú számításokat ne végezzetek bennük. Célszerű a **ModelInfo**, **VariableInfo** és **ExpressionInfo** osztályokban elvégezni a számításokat, a validációkban pedig az ezekből kinyert információk segítségével kell hibaüzenetként jelezni, ha valami nem megfelelő.*

## A névelemzés ellenőrzése

Hogy a névfeloldás és a scoping jól működik-e, könnyen ellenőrizhetitek a **Runtime Eclipse** alatt, ha a **Ctrl** billentyű lenyomása mellett rákattintotok egy változó, operáció, stb. nevére. Ilyenkor az Xtext átnavigál a név definíciójához.

A validátorok által jelzett hibákat a **Runtime Eclipse** piros aláhúzással jelzi a **.wt** fájlban.

A névfeloldás és -validáció helyes működését a **webtest.dsl.tests** projekt JUnit tesztként való futtatásával (**Run as > JUnit Test**) is ellenőrizhetitek a **NameAnalysisTests** tesztelő osztály segítségével.

## Feltöltendő

Ha ezt a részfeladatot sikerült megoldani, készítsetek screenshot-okat és töltsétek fel a képeket a saját repótokon belül a **homeworks/hw2** mappába az alábbiakról:

* A **NameAnalysisTests** összes tesztjének sikeres lefutása (mivel a név- és típuselemzés összefügg, valószínűleg a típuselemzés részfeladat megoldására is szükség lesz ehhez)
