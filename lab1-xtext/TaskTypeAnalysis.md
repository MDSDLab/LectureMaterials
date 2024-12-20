# Típuselemzés (type analysis, validation)

A típuselemzés feladata, hogy meghatározza az egyes kifejezések típusát, és ellenőrizze, hogy ezek a típusok megfelelnek-e a kód adott helyén elvárt típusnak.

Az Xtext nem végez típusellenőrzést, így ennek megvalósítása a mi feladatunk.

Ezen túlmenően hibaüzeneteket kell generálnunk akkor, ha valami probléma adódik a típusok ellenőrzésével. Ezt a validáció során tehetjük meg, amelyről részletes leírás [itt található](https://eclipse.dev/Xtext/documentation/303_runtime_concepts.html#validation).

## Xcore metamodell módosítása

Nyissátok meg a **webtest.model** projektet, és a **model** könyvtárban a **WebTest.xcore** fájlban a `Type` enumerációt módosítsátok az alábbi módon:

```Java
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

* `ERROR`: azt jelzi, hogy a típuselemzés során nem sikerült meghatározni egy adott kifejezés vagy változó típusát
* `ANY`: azt jelzi, hogy az adott helyen a kódban tetszőleges típusú kifejezés elfogadható (ilyenek pl. a `print` utasítás argumentumai)

Módosítsátok az `Expression` osztályt is az alábbi módon:
```Java
abstract class Expression
{
    Type expectedType
    Type actualType

    op void computeTypes() {}
}
```

Az `expectedType` attribútumban a kifejezés elvárt típusát, az `actualType` attribútumban pedig a kifejezés tényleges típusát kell tárolni. Ha még emlékeztek az előző félévből az előadásra: az `expectedType` egy örökölt attribútum, amelyet fentről lefelé kell kiszámolni a szintaxisfában, míg az `actualType` egy szintetizált attribútum, amelyet lentről felfelé kell kiértékelni. Ezeket a számításokat a `computeTypes()` függvény felüldefiniálásával lehet megtenni az `Expression` osztály leszármazottaiban: ki kell számolni és el kell tárolni a kifejezés aktuális típusát, valamint be kell állítani a részkifejezések elvárt típusát.

Módosítsátok a `Main` osztályt is az alábbi módon:
```Java
class Main
{
    unsettable boolean typesComputed
    String[] testClass
    contains Declaration[] declarations
    contains BlockStatement body
    
    op void computeTypes() {
        if (typesComputed) return;
        typesComputed = true
        declarations.forEach[it.computeTypes()]
        body?.computeTypes()
    }
}
```

A `Main` osztály `computeTypes()` függvényét meghívva lehet elindítani a típusszámítást a teljes szintaxisfára vonatkozóan. A `typesComputed` attribútum abban nyújt segítséget, hogy ha már egyszer le lett futtatva a típusszámítás, akkor többször már ne hajtódjon végre.

Vezessétek be a `computeTypes()` függvényt az összes többi osztályban is ( `Page`, `Operation`, `Statement` stb.).

A `computeTypes()` függvény kitöltésénél célszerű a következő sablont követni, ez éppen az attribútumok megfelelő kiértékelését adja:

```Java
class X
{
    contains Y y

    op void computeTypes() {
        this.actualType = Type.<type>     // if X is an expression
        if (y !== null) {
            y.expectedType = Type.<type>  // if Y is an expression
            y.computeTypes()
        }
    }
}
```

Például:

```Java
class IsExpression extends BinaryExpression
{
    op void computeTypes() {
        actualType = Type.BOOLEAN
        if (left !== null) {
            left.expectedType = Type.ELEMENT
            left.computeTypes()
        }
        if (right !== null) {
            right.expectedType = Type.STRING
            right.computeTypes()
        }
    }
}
```

Így végül a `Main` csúcson a `computeTypes()` függvényt meghívva a teljes szintaxisfára megtörténik a típusok számítása.


## Kifejezések tényleges és elvárt típusa

Az alábbi felsorolásban megadjuk a kifejezések tényleges típusát, illetve azt is, hogy melyik kifejezés milyen típusú részkifejezéseket vár el:

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

* `<type> <name> = <value>`: a `value` elvárt típusa `type`, a `name` által definiált változó típusa `type`
* `if <condition> then ... else ... end`: a `condition` elvárt típusa `BOOLEAN`
* `while <condition> do ... end`: a `condition` elvárt típusa `BOOLEAN`
* `<operation name> using <value>...`: a vesszőkkel elválasztott `value` értékű argumentumok elvárt típusai megegyeznek az operáció megfelelő paramétereinek típusával
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
* `test <name>(<parameter>...) with <value>... <statement>... end`: a vesszőkkel elválasztott `value` értékű argumentumok elvárt típusai megegyeznek az teszteset megfelelő paramétereinek típusával

## Validation

A **webtest.dsl** projekten belül az **src** könyvtár alatt a **webtest.dsl.validation** csomagban hozzatok létre egy **TypeValidator.java** fájlt, hasonlóan a névelemzésnél használt **NameValidator.java** fájlhoz.

A **webtest.dsl** projekten belül az **src** könyvtár alatt a **webtest.dsl.scoping** csomagból nyissátok meg a **WebTestDslValidator.java** fájlt, és regisztráljátok be ezt a **TypeValidator** osztályt is az alábbi módon:

```Java
package webtest.dsl.validation;

import org.eclipse.xtext.validation.ComposedChecks;

/**
 * This class contains custom validation rules. 
 *
 * See https://www.eclipse.org/Xtext/documentation/303_runtime_concepts.html#validation
 */
@ComposedChecks(validators = {NameValidator.class, TypeValidator.class})
public class WebTestDslValidator extends AbstractWebTestDslValidator {
	
}

```

A **TypeValidator** osztályban készítsétek el az alábbi ellenőrzést:

* Jelezzétek, ha egy kifejezés tényleges típusa nem felel meg a kifejezés elvárt típusának: `"Expression of type <expected type> expected but <actual type> was found."`. A behelyettesített típusnevek az alábbiak lehetnek: `STRING`, `INTEGER`, `BOOLEAN`, `ELEMENT` vagy `UNDEFINED`.

## A típuselemzés ellenőrzése

A validátorok által jelzett hibákat a **Runtime Eclipse** piros aláhúzással jelzi a **.wt** fájlban.

A típuselemzés és -validáció helyes működését a **webtest.dsl.tests** projekt JUnit tesztként való futtatásával (**Run as > JUnit Test**) is ellenőrizhetitek a **TypeAnalysisTests** és a **TypeAnalysisExtensionsTests** tesztelő osztályok segítségével.

## Feltöltendő

Ha ezt a részfeladatot sikerült megoldani, készítsetek screenshot-okat és töltsétek fel a képeket a saját repótokon belül a **homeworks/hw1** mappába az alábbiakról:

* A **TypeAnalysisTests** és a **TypeAnalysisExtensionsTests** összes tesztjének sikeres lefutása.
