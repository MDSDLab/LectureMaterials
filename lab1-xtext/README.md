# 1. labor - Xcore metamodell, Xtext nyelvtan, névelemzés, típuselemzés

## A laborfeladat elvégzésének lépései

0. Töltsétek le, és telepítsétek az alábbi szoftvereket:

* [Eclipse IDE for Java and DSL Developers 2024-06 R](https://www.eclipse.org/downloads/packages/release/2024-06/r/eclipse-ide-java-and-dsl-developers). FONTOS: pontosan ezt a változatot és ezt a verziót használjátok, mert a projektek csak ezzel fordulnak helyesen!
* [Maven](https://maven.apache.org/download.cgi). Adjátok hozzá a Maven `bin` könyvtárát a `PATH` környezeti változóhoz.
* JDK 21 (pl. [Adoptium](https://adoptium.net/)). A `JAVA_HOME` környezeti változó a telepített JDK főkönyvtárára mutasson.

1. Csináljátok végig az [Xcore Wiki](https://wiki.eclipse.org/Xcore) és az [Xtext](https://eclipse.dev/Xtext/documentation/102_domainmodelwalkthrough.html) oldalakon található tutorialokat! Olvassátok el még egyszer a tavalyi [Xtext gyakorlat](images/GY3-Xtext-Utmutato.pdf) anyagát!
2. Az [mdsd-2024-lab1-xtext](https://github.com/MDSDLab/mdsd-2024-lab1-xtext) repóból másoljátok át a **webtest.dsl.parent** könyvtárat közvetlenül a saját repótok gyökerébe!
3. Oldjátok meg az alábbiakban leírt feladatokat, figyelembe véve a [számotokra kiosztott bővítményeket](ExtrasTable2024.md)!
4. Beadás előtt ellenőrizzétek, hogy a **webtest.dsl.parent** könyvtárból a `compile.bat` parancs sikeresen lefut-e. Minden projektnek hiba nélkül le kell fordulnia.
5. Készítsetek egy **hw1** nevű git tag-et az utolsó commitra!

## Feladat célja

A laborfeladat célja a [WebTest nyelv](WebTestLanguageSpecification.md) metamodelljének kialakítása, valamint a nyelv fordítójának elkészítése név- és típuselemzéssel együtt.

A WebTest nyelv [közös részét](WebTestReference.md) mindenkinek meg kell valósítania. A [bővítmények](WebTestReferenceExtra.md) közül csak a kiosztott 2 darab bővítményt kell kötelezően támogatni, a többi bővítmény támogatása opcionális.

## Feladat leírása

Nyissátok meg a **webtest.dsl.parent** könyvtár alatt található projekteket Eclipse alatt: **File > Import... / General > Existing Projects into Workspace** (nem Maven > Existing Maven Projects!), és **root directory**-nak a **webtest.dsl.parent**-et válasszátok! A megnyitás után előfordulhatnak fordítási hibák a **webtest.dsl.tests** és a **webtest.model.tests** projektekben. Ezek a hibák azért jelennek meg, mert a laborfeladat során kiegészítendő fájlok tartalma még nem teljes. A hibáknak el kell tűnnie a laborfeladat megoldásának végére. A többi projektnek hiba nélkül kell betöltődnie.

A **webtest.dsl** projektben az **src** könyvtár alatt a **webtest.dsl** csomagban szereplő **WebTestExtensions** osztályban állítsátok **false**-ra az azon bővítményekhez tartozó értékeket, amelyeket nem kell megvalósítanotok. Így csak olyan tesztek lesznek engedélyezve, amelyek számotokra relevánsak.

FONTOS: Az **xtend** kiterjesztésű fájlok csak az Eclipse IDE-ben dolgozva látják az azonos projektben lévő **java** kiterjesztésű fájlokat, a Maven fordítás során nem! Emiatt célszerű ezen a laboron az **xtend** kiterjesztésű fájlokat mellőzni. Ha mégis használtok ilyen fájlokat, győződjetek meg róla, hogy a Maven fordítás is működőképes-e!

Oldjátok meg az alábbi feladatokat:

0. [Mintapélda kipróbálása](TaskExample.md)
1. [Metamodell elkészítése](TaskMetaModel.md) (5 pont)
2. [Nyelvtan elkészítése](TaskGrammar.md) (10 pont)
3. [Névelemzés](TaskNameAnalysis.md) (5 pont)
4. [Típuselemzés](TaskTypeAnalysis.md) (5 pont)

## Megoldás ellenőrzése

A **webtest.model.tests** projekt azt teszteli, hogy az elkészített Xcore metamodell megfelel-e a laborfeladat követelményeinek.

A **webtest.dsl.tests** projekt azt teszteli, hogy az elkészített Xtext nyelvtan, valamint a hozzá tartozó név- és típuselemzés megfelel-e a laborfeladat követelményeinek.

A laborfeladat megoldása akkor tekinthető késznek, ha az összes releváns teszt sikeresen le tud futni, és a Maven fordítás is sikeres a `compile.bat` segítségével. Azonban a laborfeladat megoldásának alaposabb kiértékeléséhez a itt szereplő (előre kiadott) teszteken túl további tesztek is futtatásra kerülhetnek a leadás után!

## Referenciák

Hasznos linkek a feladat megoldásához:

* [Xcore Wiki](https://wiki.eclipse.org/Xcore)
* [Xtext](https://eclipse.dev/Xtext/documentation/index.html)
* [Xtend](https://eclipse.dev/Xtext/xtend/documentation/index.html)
* Az előző féléves Xtext-Xtend gyakorlat [útmutatója](images/GY3-Xtext-Utmutato.pdf)
* Az Xcore operációk belsejében Xbase kifejezések használhatók. Az Xtend nyelv is Xbase kifejezéseket használ, így az Xbase nyelv referenciájaként leginkább a következő oldal szolgálhat: [Xtend Expressions](https://eclipse.dev/Xtext/xtend/documentation/203_xtend_expressions.html)

