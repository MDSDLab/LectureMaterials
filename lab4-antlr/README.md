# 4. labor (1. rész) - ANTLR fordító és kódgenerátor

## A laborfeladat elvégzésének lépései

1. Az [mdsd-2025-lab4-antlr](https://github.com/MDSDLab/mdsd-2025-lab4-antlr) repó tartalmát másoljátok be a saját repótokba, egy újonnan létrehozott **webtest-input-validation** mappába!
2. Oldjátok meg az alábbiakban leírt feladatokat, figyelembe véve a számotokra kiosztott bővítményt!
3. Készítsetek egy **hw4-antlr** nevű **tag**-et az utolsó commitra!

## HTML form validációt leíró nyelv (WebTestInputValidation) készítése

A laborfeladat célja ANTLR segítségével egy új, a WebTest nyelvtől különálló, de azt támogató WebTestInputValidation nyelv, valamint a hozzá tartozó kódgenerátorok elkészítése. A fordító feladata, hogy a bemeneti (.wtiv kiterjesztésű) fájlokból egy belső adatstruktúrát építsen fel, valamint megtalálja és jelezze a felhasználónak a szemantikai hibákat (szemantikai elemzés). Ezek után a bemeneti fájlból pontosan egy darab HTML és egy darab JavaScript kimeneti fájlt kell, hogy generáljon, melyeken a gyakorlatban futtathatók a korábban létrehozott, WebTest nyelven leírt tesztek. A tesztek futtatása nem képezi a feladat részét (ez csak a gyakorlati alkalmazása lenne az elkészült nyelvnek), tehát ha a 2. labor során nem sikerült megvalósítani a kódgenerátort, az a 4. labornál nem jár semmiféle hátránnyal, így pontlevonással sem.

A WebTestInputValidation nyelv közös részét mindenkinek meg kell valósítania. A kiadott bemeneti példakódban a közös részeken kívül minden bővítményre található példa (kommenttel megjelölve), viszont minden csapatnak csak 1 darab bővítményt kell támogatni, a többi bővítmény támogatása opcionális.

## Kiinduló projekt módosítása
A kiinduló projekt egy vázat ad a megoldás elkészítéséhez. A váz csak egy ajánlás, azt módosítani, osztályokat hozzáadni vagy törölni, stb. bármilyen mértékben szabad, egy megkötéssel: a **WebtestInputRunner** osztály main metódusát futtatva le kell futnia a szemantikai elemzésnek és a kódgenerálásnak is!

## Feladat leírása

A laborfeladat elvégzéséhez nyissátok meg a kiinduló projektet [IntelliJ IDEA](https://www.jetbrains.com/idea/)-val.

Előkészületek:

0. [A projekt előkészítése](Technical.md)

A [WebtestInputValidation](WebTestInputValidationSpecification.md) nyelv elkészítése:

1. [A nyelvtan elkészítése](Grammar.md)
2. [A mögöttes adatstruktúra felépítése](DataStructure.md)
3. [Szemantikai elemzés](SemanticAnalysis.md)

Kódgenerálás:

4. [HTML kódgenerálás](HTMLGeneration.md)
5. [JavaScript kódgenerálás](JSGeneration.md)

Bővítmények:

6. [Bővítmények leírása](Extensions.md)
7. [Bővítmények kiosztása](ExtensionsTable.md)

## Értékelés

A kiinduló projektben (**examples** package) található egy bemeneti [példakód](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/PersonForm.wtiv), melyben a WebestInputValidation nyelv minden nyelvi eleme megtalálható. Szintén itt (**examples.generated** package) találhatók meg a példakódból generált [HTML](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/generated/PersonForm.html) és [JavaScript](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/generated/validation.js) fájlok is. A nyelvtan elkészítése és a kódgenerálás akkor tekinthető teljes értékűnek, ha a bemeneti fájlt sikerül teljesen beolvasni és értelmezni, valamint ha a fentiekkel funkcionálisan ekvivalens kódot sikerül belőle generálni. A szemantikai elemzés akkor tekinthető teljes értékűnek, ha minden, a feladatkiírásban leírt esetnél keletkezik egy megfelelő hiba.

A megoldást akár a szintén az **examples** package-ben található, WebTest nyelven írt [példához](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/FormTest.wt) hasonló, automatikusan generált Selenium tesztekkel is ellenőrizhetjük.

### Pontozás

A 4. labor két különálló feladatból áll; az **ANTLR feladatra 15 pontot**, a **[Blockly](https://github.com/MDSDLab/LectureMaterials/blob/main/lab4-blockly/README.md) feladatra 10 pontot** lehet maximum szerezni. Az ANTLR feladat pontozása a következőképpen oszlik el:

| Nyelvtan | Szemantikai elemzés | Kódgenerálás |
| :-: | :-: | :-: |
| 3 pont | 3 pont | 9 pont |



## Referenciák

Hasznos linkek a feladat elvégzéséhez:

* Az előző féléves ANTLR gyakorlatok anyagai - lexikai / szintaktikai elemzés [gyakorlat](https://github.com/bmeaut/ModellalapuSzoftverfejlesztes/tree/master/practice/practice_02) + szemantikai elemzés / kódgenerálás [gyakorlat](https://github.com/bmeaut/ModellalapuSzoftverfejlesztes/tree/master/practice/practice_04)
    * IntelliJ és ANTLR beüzemelési útmutatót is tartalmaz
* [ANTLR](https://www.antlr.org/) ([Dokumentáció](https://github.com/antlr/antlr4/blob/master/doc/index.md))
* [ANTLR példa nyelvtanok](https://github.com/antlr/grammars-v4)
* [StringTemplate](https://www.stringtemplate.org/) ([Dokumentáció](https://github.com/antlr/stringtemplate4/blob/master/doc/index.md))
