# Típuselemzés (type analysis, validation)

A típuselemzés feladata, hogy meghatározza az egyes kifejezések típusát, kikövetkeztesse a változók és paraméterek típusát, és ellenőrizze, hogy az egyes kifejezések típusai megfelelnek-e a kód adott helyén elvárt típusnak.

Az Xtext nem végez típusellenőrzést, így ennek megvalósítása a mi feladatunk. Felhasználhatjuk ehhez az Xcore metamodellhez csatolt extra információkat is.

Ezen túlmenően hibaüzeneteket kell generálnunk akkor, ha valami probléma adódik a típusok ellenőrzésével. Ezt a validáció során tehetjük meg, amelyről részletes leírás [itt található](https://eclipse.dev/Xtext/documentation/303_runtime_concepts.html#validation).

## Xcore metamodell módosítása

Nyissátok meg a **webtest.model** projektet, és a **model** könyvtárban a **WebTest.xcore** fájlban a **Type** enumerációt módosítsátok az alábbi módon:

```
enum Type
{
    UNDEFINED,
    ERROR,
    ANY,
    STRING,
    INTEGER,
    BOOLEAN,
    ELEMENT
}
```

A korábbi értékek mellé bekerült két új érték:

* `ERROR`: azt jelzi, hogy a típuselemzés során nem sikerült kikövetkeztetni egy adott kifejezés vagy változó típusát
* `ANY`: azt jelzi, hogy az adott helyen a kódban tetszőleges típusú kifejezés elfogadható (ilyenek pl. a `print` utasítás argumentumai)

## Kifejezések tényleges és elvárt típusa

A kifejezések elvárt típusát az Xcore metamodellhez csatolt extra információk segítségével lehet kiszámolni.

A kifejezések tényleges típusát az Xcore metamodellben már meghatároztuk az **Expression.getType()** függvényen keresztül. A teljesség kedvéért az alábbi felsorolásban megadjuk ezeket, illetve azt is, hogy melyik kifejezés milyen típusú részkifejezéseket vár el:

* `<elem> is <value>`: az `elem` elvárt típusa `ELEMENT`, a `value` elvárt típusa `STRING`, a teljes kifejezés tényleges típusa `BOOLEAN`
* `<elem> contains <value>`: az `elem` elvárt típusa `ELEMENT`, a `value` elvárt típusa `STRING`, a teljes kifejezés tényleges típusa `BOOLEAN`
* `<elem> exists`: az `elem` elvárt típusa `ELEMENT`, a teljes kifejezés tényleges típusa `BOOLEAN`
* `not <value>`: a `value` elvárt típusa `BOOLEAN`, a teljes kifejezés tényleges típusa `BOOLEAN`
* karakterlánc konstans kifejezés: tényleges típusa `STRING`
* egész szám konstans kifejezés: tényleges típusa `INTEGER`
* logikai érték konstans kifejezés: tényleges típusa `BOOLEAN`
* HTML elem konstans kifejezés: tényleges típusa `ELEMENT`
* változó hivatkozás mint kifejezés: tényleges típusa a változó tényleges típusa

Az alábbi felsorolásban megadjuk, hogy melyik utasítás milyen típusú kifejezéseket vár el:

* `operation <name> using <parameter>... <statement>... end`: a paraméterek tényleges típusát az operáció törzse alapján kell kikövetkeztetni (ld. következő szakasz)
* `set <name> to <value>`: a `value` elvárt típusa `ANY`, a `name` által definiált változó típusa megegyezik a `value` típusával
* `if <condition> then ... else ... end`: a `condition` elvárt típusa `BOOLEAN`
* `while <condition> do ... end`: a `condition` elvárt típusa `BOOLEAN`
* `<operation name> using <value>...`: a vesszőkkel elválasztott `value` értékű argumentumok elvárt típusai megegyeznek az operáció megfelelő paramétereinek típusával
* `<operation name> using <name>:<value>...`: a vesszőkkel elválasztott `name` nevű `value` értékkel rendelkező argumentumok elvárt típusai megegyeznek az operáció megfelelő nevű paramétereinek típusával
* `open <url>`: az `url` elvárt típusa `STRING`
* `fill <elem> with <text>`: az `elem` elvárt típusa `ELEMENT`, a `text` elvárt típusa `STRING`
* `click <elem>`: az `elem` elvárt típusa `ELEMENT`
* `context <elem> as <page> ... end`: az `elem` elvárt típusa `ELEMENT`
* `print <value>...`: a vesszőkkel elválasztott `value` értékek elvárt típusa `ANY`
* `assert <condition>`: a `condition` elvárt típusa `BOOLEAN`
* `wait <time> seconds until <condition>`: a `time` elvárt típusa `INTEGER`, a `condition` elvárt típusa `BOOLEAN`

A bővítményként megvalósítandó utasítások esetén a kifejezések elvárt típusai a következők:

* `capture <elem>`: az `elem` elvárt típusa `ELEMENT`
* `javascript <code> using <value>...`: az `code` elvárt típusa `STRING`, a vesszőkkel elválasztott `value` értékek elvárt típusa `ANY`
* `foreach <item> in <items>`: az `item` tényleges típusa `ELEMENT`, az `items` elvárt típusa `ELEMENT`
* `test <name> using <parameter>... with <value>... <statement>... end`: a vesszőkkel elválasztott `value` értékű argumentumok elvárt típusai megegyeznek az teszteset megfelelő paramétereinek típusával, a paraméterek tényleges típusát a teszteset törzse alapján kell kikövetkeztetni (ld. következő szakasz)
* `test <name> using <parameter>... with <name>:<value>... <statement>... end`: a vesszőkkel elválasztott `name` nevű `value` értékkel rendelkező argumentumok elvárt típusai megegyeznek a teszteset megfelelő nevű paramétereinek típusával, a paraméterek tényleges típusát a teszteset törzse alapján kell kikövetkeztetni (ld. következő szakasz)

## Változók és paraméterek típusának kikövetkeztetése

A WebTest nyelvben nem kell explicit kiírni a változók és a paraméterek típusát. A típust a fordítónak kell automatikusan kikövetkeztetnie attól függően, hogy a változó definiálásakor milyen típusú értékkel van inicializálva, illetve hogy a paraméter milyen elvárt típussal rendelkezik az egyes utasításokban és kifejezésekben.

Az előző szakasz megadta az egyes kifejezések tényleges és elvárt típusát. Ezeket lehet felhasználni a típuskikövetkeztetés során.

Amennyiben a kikövetkeztetett típus nem egyértelmű (pl. a paraméternek `INTEGER` és `ELEMENT` típusúnak is kell lennie egyszerre), vagy nincs elég információ a kikövetkeztetéshez (pl. az adott paramétert sosem használjuk az operáción belül), akkor `ERROR` lesz a kikövetkeztetett típus.

Ha `ANY` és egy konkrét típus (`STRING`, `INTEGER`, `BOOLEAN` vagy `ELEMENT`) is szóba jöhet a kikövetkeztetésnél, akkor a konkrét típus nyer, az lesz a kikövetkeztetett típus, mert az a speciálisabb.

***TIPP:** A típuskikövetkeztetést célszerű az Xcore objektumokhoz csatolt **ModelInfo**,  **VariableInfo** és **ExpressionInfo** osztályok segítségével megvalósítani, de létrehozhattok további segédosztályokat is. A modell teljes bejárásával meg kell vizsgálni az összes helyet, ahol kifejezés szerepel, be kell állítani az összes kifejezés elvárt típusát az **ExpressionInfo**-ban. Ahol a kifejezés egy változóra történő hivatkozás, ott a kifejezés elvárt típusa egyben meghatározza a változó kikövetkeztetett típusát is, amit a megfelelő **VariableInfo** objektumban elmenthetünk.*

*A modell bejárásában sok segítséget nyújthatnak az alábbi konstrukciók:*

* *Az Xtend nyelv [dispatch](https://eclipse.dev/Xtext/xtend/documentation/202_xtend_classes_members.html#polymorphic-dispatch) függvénydeklarációja, amelyet meghívva futásidőben a dinamikus típus dől el, hogy melyik overload változat hívódik meg, így nem kell **instanceof**-ot használnunk. A dispatch függvényeket statikus függvényként is definiálhatjuk, ekkor úgy használhatók, mint C#-ban az extension method-ok, de továbbra is dinamikusan, futásidőben dől el, melyik dispatch változat hívódik meg az overload-ok közül.*
* *Az **EcoreUtil2** osztály **getContainerOfType** függvénye megadja az adott modellbeli objektum legközelebbi olyan tartalmazó objektumát, amely az általunk kívánt típusnak megfelel. Ha az objektum maga már a kívánt típusú, akkor magával az objektummal tér vissza.*
* *Az **EcoreUtil2** osztály **getAllContentsOfType** függvénye megadja az adott modellbeli objektum által tartalmazott összes olyan objektumot, amely az általunk kívánt típusnak megfelel.*
* *Használhattok **instanceof**-ot is, ha szükséges.*

## Validation

A **webtest.dsl** projekten belül az **src** könyvtár alatt a **webtest.dsl.validation** csomagban hozzatok létre egy **TypeValidator.xtend** fájlt, a következő tartalommal:

```
package webtest.dsl.validation

import org.eclipse.xtext.validation.Check
import org.eclipse.xtext.validation.EValidatorRegistrar
import webtest.model.Type
import webtest.model.Variable

class TypeValidator extends AbstractWebTestDslValidator {
    // This empty override is necessary to prevent reporting duplicate validation messages
    override register(EValidatorRegistrar registrar) {
    }

    @Check(NORMAL)
    def checkInferredType(Variable variable) {
        try {
            val inferred = variable.type
            if (inferred === null || inferred == Type.UNDEFINED || inferred == Type.ERROR) {
                error("Could not infer the type of the variable '"+variable.name+"'.", variable, null)
            }
        } catch (Exception ex) {
            ex.printStackTrace
        }
    }    
}
```

Ez a **checkInferredType** függvényben ellenőrzi, hogy sikerült-e minden változó és paraméter típusát kikövetkeztetni. Amennyiben nem, hibaüzenetet készít a problémás változókhoz és paraméterekhez: `Could not infer the type of the variable '<name>'.`

Még egy lépést el kell végeznünk, hogy a fenti validáció lefusson. A **webtest.dsl** projekten belül az **src** könyvtár alatt a **webtest.dsl.scoping** csomagból nyissátok meg a **WebTestDslValidator.xtend** fájlt, és módosítsátok a tartalmát az alábbi módon:

```
package webtest.dsl.validation

import org.eclipse.xtext.validation.ComposedChecks

/**
 * This class contains custom validation rules. 
 *
 * See https://www.eclipse.org/Xtext/documentation/303_runtime_concepts.html#validation
 */
@ComposedChecks(validators=#[NameValidator, TypeValidator])
class WebTestDslValidator extends AbstractWebTestDslValidator {

}
```

Így a **WebTestDslValidator** a **TypeValidator** osztályt is használni fogja validálásra.

A laborfeladat során fejlesszétek tovább a **TypeValidator** osztályt:

* Jelezzétek, ha egy kifejezés tényleges típusa nem felel meg a kifejezés elvárt típusával: `"Expression of type <expected type> expected but <actual type> was found."`. A behelyettesített típusnevek az alábbiak lehetnek: `STRING`, `INTEGER`, `BOOLEAN` vagy `ELEMENT`.

***TIPP:** Az elvárt típust számíthatjátok a kifejezésekhez csatolt **ExpressionInfo** objektumokban, ahogy az előző szakasz leírta. A tényleges típust az Xcore metamodellben már leírtuk. A validációban csak ezeket kell egymással összeegyeztetni.*

## A típuselemzés ellenőrzése

A validátorok által jelzett hibákat a **Runtime Eclipse** piros aláhúzással jelzi a **.wt** fájlban.

A típuselemzés és -validáció helyes működését a **webtest.dsl.tests** projekt JUnit tesztként való futtatásával (**Run as > JUnit Test**) is ellenőrizhetitek a **TypeAnalysisTests** tesztelő osztály segítségével.

## Feltöltendő

Ha ezt a részfeladatot sikerült megoldani, készítsetek screenshot-okat és töltsétek fel a képeket a saját repótokon belül a **homeworks/hw2** mappába az alábbiakról:

* A **TypeAnalysisTests** összes tesztjének sikeres lefutása (mivel a név- és típuselemzés összefügg, valószínűleg a névelemzés részfeladat megoldására is szükség lesz ehhez)
