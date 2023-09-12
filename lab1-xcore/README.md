# 1. labor - Xcore metamodell

## A laborfeladat elvégzésének lépései

1. Csináljátok végig az [Xcore Wiki](https://wiki.eclipse.org/Xcore) oldalon található tutorialt!
2. Az [mdsd-2023-lab1-xcore](https://github.com/MDSDLab/mdsd-2023-lab1-xcore) repóból másoljátok át a **webtest.model** és **webtest.model.tests** projekteket közvetlenül a saját repótok gyökerébe!
3. Nyissátok meg a **webtest.model** és **webtest.model.tests** projekteket Eclipse alatt!
4. Oldjátok meg az alábbiakban leírt feladatokat, figyelembe véve a [számotokra kiosztott bővítményeket](ExtrasTable2023.md)!
5. Készítsetek egy **hw1** nevű **tag**-et az utolsó commitra!

## Metamodell készítése a WebTest nyelvhez

A laborfeladat célja a [WebTest nyelv](WebTestLanguageSpecification.md) metamodelljének kialakítása. Ez adja az alapját az Xtext fordítónak és az Xtend kódgenerátornak, amelyeket egy későbbi laborfeladat során kell megvalósítani.

A metamodell elkészítése során objektum-orientált módon kell megtervezni egy olyan UML osztálydiagram-szerű modellt, amely képes a WebTest nyelv minden elemét reprezentálni.

A metamodell kitalálása is lehetne egy önálló laborfeladat, de az egységesség és az automatizált tesztelhetőség kevéért megadjuk az általunk megtervezett metamodell grafikus reprezentációját UML osztálydiagram formájában. [Ezt a grafikus reprezentációt](MetaModel.md) kell a laborfeladat során leképezni Xcore reprezentációra.

A WebTest nyelv [közös részét](WebTestReference.md) mindenkinek meg kell valósítania. A [bővítmények](WebTestReferenceExtra.md) közül csak a kiosztott 2 darab bővítményt kell kötelezően támogatni, a többi bővítmény támogatása opcionális.

## Feladat leírása

A **webtest.model** projekten belül a **model/webtest.xcore** fájlban kell dolgozni, ide kell beírni a [metamodell](MetaModel.md) Xcore reprezentációját.

A megoldás során az alábbi fázisokról készítsetek screenshot-okat és töltsétek fel a képeket a saját repótokon belül a **homeworks/hw1** mappába:

* Az **IfStatement** Xcore reprezentációja.
* A kiosztott két bővítmény Xcore reprezentációja.

## Megoldás ellenőrzése

A **webtest.model.tests** projekt azt teszteli, hogy az elkészített Xcore metamodell megfelel-e a kiadott osztálydiagramoknak. A projektben az alábbi JUnit tesztosztályok szerepelnek:

* **MetaModelTests**: a közös metamodell leképzését ellenőrzi
* **BasePagesTests**: a **BasePages** nevű bővítmény leképzését ellenőrzi
* **CaptureTests**: a **Capture** nevű bővítmény leképzését ellenőrzi
* **ForEachTests**: a **ForEach** nevű bővítmény leképzését ellenőrzi
* **JavaScriptTests**: a **JavaScript** nevű bővítmény leképzését ellenőrzi
* **ManualTests**: a **Manual** nevű bővítmény leképzését ellenőrzi
* **TestParamsTests**: a **TestParams** nevű bővítmény leképzését ellenőrzi

A bővítményeket tesztelő osztályokból csak azokat kell megtartani, amelyek a kiosztott 2 darab bővítményt tesztelik, a többi bővítményt tesztelő osztály belseje kikommentezhető.

A **MetaModelTests** osztályt és belsejét mindenképpen meg kell tartani.

A WebTest metamodell leképzése és így a laborfeladat megoldása akkor tekinthető sikeresnek, ha az összes releváns teszt sikeresen le tud futni. Azonban a laborfeladat megoldásának alaposabb kiértékeléséhez a itt szereplő (előre kiadott) teszteken túl további tesztek is futtatásra kerülnek leadás után!

A következő fázisokról készítsetek screenshot-okat és töltsétek fel a képeket a saját repótokon belül a **homeworks/hw1** mappába:

* A **webtest.model.tests** projektben található releváns JUnit tesztek sikeres lefutását összegző Eclipse ablak.

## Referenciák

Hasznos linkek az Xcore használatához:

* [Xcore Wiki](https://wiki.eclipse.org/Xcore)
* Az előző féléves Xtext-Xtend gyakorlat [útmutatója](images/GY4-XtextXtend-Utmutato.pdf)
* Az Xcore operációk belsejében Xbase kifejezések használhatók. Az Xtend nyelv is Xbase kifejezéseket használ, így az Xbase nyelv referenciájaként leginkább a következő oldal szolgálhat: [Xtend Expressions](https://eclipse.dev/Xtext/xtend/documentation/203_xtend_expressions.html)

