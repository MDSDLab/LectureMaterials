# 1. labor - Xcore metamodell, Xtext nyelvtan, névelemzés, típuselemzés

## A laborfeladat elvégzésének lépései

1. Csináljátok végig az [Xcore Wiki](https://wiki.eclipse.org/Xcore) és az [Xtext](https://eclipse.dev/Xtext/documentation/102_domainmodelwalkthrough.html) oldalakon található tutorialokat!
2. Az [mdsd-2024-lab1-xcore-xtext](https://github.com/MDSDLab/mdsd-2024-lab1-xcore-xtext) repóból másoljátok át a **webtest.parent** könyvtárat közvetlenül a saját repótok gyökerébe!
3. Oldjátok meg az alábbiakban leírt feladatokat, figyelembe véve a [számotokra kiosztott bővítményeket](ExtrasTable2024.md)!
4. Készítsetek egy **hw1** nevű **tag**-et az utolsó commitra!

## Feladat célja

A laborfeladat célja a [WebTest nyelv](WebTestLanguageSpecification.md) metamodelljének kialakítása, valamint a nyelv fordítójának elkészítése név- és típuselemzéssel együtt.

A WebTest nyelv [közös részét](WebTestReference.md) mindenkinek meg kell valósítania. A [bővítmények](WebTestReferenceExtra.md) közül csak a kiosztott 2 darab bővítményt kell kötelezően támogatni, a többi bővítmény támogatása opcionális.

## Feladat leírása

Nyissátok meg a **webtest.parent** könyvtár alatt található projekteket Eclipse alatt! A **webtest.dsl** projektben az **src** könyvtár alatt a **webtest.dsl** csomagban szereplő **WebTestExtensions** osztályban állítsátok **false**-ra az azon bővítményekhez tartozó értékeket, amelyeket nem kell megvalósítanotok. Így csak olyan tesztek lesznek engedélyezve, amelyek számotokra relevánsak.

Oldjátok meg az alábbi feladatokat:

0. [Mintapélda kipróbálása](TaskExample.md)
1. [Metamodell elkészítése](TaskMetaModel.md)
2. [Nyelvtan elkészítése](TaskGrammar.md)
3. [Névelemzés](TaskNameAnalysis.md)
4. [Típuselemzés](TaskTypeAnalysis.md)

## Referenciák

Hasznos linkek az Xcore használatához:

* [Xcore Wiki](https://wiki.eclipse.org/Xcore)
* [Xtext](https://eclipse.dev/Xtext/documentation/index.html)
* [Xtend](https://eclipse.dev/Xtext/xtend/documentation/index.html)
* Az előző féléves Xtext-Xtend gyakorlat [útmutatója](images/GY4-XtextXtend-Utmutato.pdf)
* Az Xcore operációk belsejében Xbase kifejezések használhatók. Az Xtend nyelv is Xbase kifejezéseket használ, így az Xbase nyelv referenciájaként leginkább a következő oldal szolgálhat: [Xtend Expressions](https://eclipse.dev/Xtext/xtend/documentation/203_xtend_expressions.html)

