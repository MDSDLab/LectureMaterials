# Mintapélda kipróbálása

A **webtest.transformation** projekt **queries** mappáján belül a **webtest.transformation.queries** package-ben lévő **queries.vql** fájlban található egy példa lekérdezés. Az **src** mappában, a **webtest.transformation.rules** package-ben lévő **MutationRules.xtend** fájlban található egy példa transzformációs szabály.

A webtest.transformation projekten belül az **example** és **error-example** könyvtárakban találhatók példa modellek, melyeken a lekérdezéseket és transzformációkat ki tudjátok próbálni.

## Lekérdezések futtatása
A Viatra lekérdezések eredményének megtekintésére használhatjátok a Query Results nézetet. Ezt a **Window -> Show View -> Other...** menüpontban, a **Viatra -> Query Results** kiválasztásával tudjátok megnyitni. Ahhoz, hogy a lekérdezések megfelelően működjenek, be kell kapcsolni a **Dynamic EMF mode**-ot a **Window -> Preferences -> VIATRA -> Query Evaluation** menüpontban.

Nyissátok meg az error-examples mappában található **calculator-1.xml** fájlt a **Sample Reflective Ecore Model Editor**-ral, és a Query Results nézet jobb felső sarkában kattintsatok a zöld háromszögre (**Load model from active editor**) a modell betöltéséhez, majd nyissátok meg a **queries.vql** fájlt, és kattintsatok az újonnan kizöldült háromszögre (**Load queries from active editor**) a lekérdezések betöltéséhez. A lekérdezés eredménye megjelenik a Query Results nézetben. (Ha mégsem jelenik meg vagy nem frissül az eredmény, szerkesszétek és mentésétek a vql fájlt, például egy üres sor beszúrásával, majd egyből kattintsatok a **Load queries from active editor** gombra.)

## Lekérdezés tesztek futtatása
Néhány generált teszt található a **test** mappán belül a **webtest.transformation.querytest** package-ben lévő **QueryTests.xtend** fájlban. Ezek futtatásához a **Jobb klikk -> Run As -> JUnit Test** menüpontot kell használni.

## Transzformációk futtatása
A transzformációkat a **MutationRules.xtend** fájl futtatásával tudjátok kipróbálni: **Jobb klikk -> Run as -> Java Application**. Ez betölti a megadott modellt, elvégez N darab (alapértelmezetten 2) véletlenszerűen kiválasztott transzformációt, majd a módosított modellt elmenti a megadott mappába. Ez alapértelmezetten az **output** mappa, újbóli futtatás esetén az alkalmazás a korábbi mutánst felülírja. Amennyiben nem tapasztaltok változást a megnyitott modellben, frissítsétek a fájlt.