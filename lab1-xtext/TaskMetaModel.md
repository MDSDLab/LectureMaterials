# Metamodell elkészítése

## Feladat

A metamodell elkészítése során objektum-orientált módon kell megtervezni egy olyan UML osztálydiagram-szerű modellt, amely képes a WebTest nyelv minden elemét reprezentálni.

A metamodell kitalálása is lehetne egy önálló laborfeladat, de az egységesség és az automatizált tesztelhetőség kevéért megadjuk az [általunk megtervezett metamodell](MetaModel.md) grafikus reprezentációját UML osztálydiagram formájában. Ezt a grafikus reprezentációt kell a laborfeladat során leképezni Xcore reprezentációra.

A **webtest.model** projekten belül a **model/webtest.xcore** fájlban kell dolgozni, ide kell beírni a metamodell Xcore reprezentációját.

## Ellenőrzés

A **webtest.model.tests** projekt azt teszteli, hogy az elkészített Xcore metamodell megfelel-e a kiadott osztálydiagramoknak. A projektben az alábbi JUnit tesztosztályok szerepelnek:

* **MetaModelTests**: a közös metamodell leképzését ellenőrzi
* **CaptureTests**: a **Capture** nevű bővítmény leképzését ellenőrzi
* **ForEachTests**: a **ForEach** nevű bővítmény leképzését ellenőrzi
* **JavaScriptTests**: a **JavaScript** nevű bővítmény leképzését ellenőrzi
* **ManualTests**: a **Manual** nevű bővítmény leképzését ellenőrzi
* **TestParamsTests**: a **TestParams** nevű bővítmény leképzését ellenőrzi

A bővítményeket tesztelő osztályokból csak azokat kell megtartani, amelyek a kiosztott 2 darab bővítményt tesztelik, a többi bővítményt tesztelő osztály belseje kikommentezhető.

A **MetaModelTests** osztályt és belsejét mindenképpen meg kell tartani.

## Feltöldendő

A megoldás során az alábbi fázisokról készítsetek screenshot-okat és töltsétek fel a képeket a saját repótokon belül a **homeworks/hw1** mappába:

* Az **IfStatement** Xcore reprezentációja.
* A kiosztott két bővítmény Xcore reprezentációja.
* A **webtest.model.tests** projektben található releváns JUnit tesztek sikeres lefutását összegző Eclipse ablak.

