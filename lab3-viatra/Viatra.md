# A VAITRA lekérdezések elkészítése
Modellekérdezésre több okból is szükségünk lehet. A WebTest nyelv metamodellje nem kényszeríti ki a hibamentes modellek készítését, ezért jólformáltsági kényszerek szükségesek. Ezen kényszerek ellenőrzéséhez meg kell adnunk hibamintákat, melyek jelzik a hibás modelleket. A hibaminták felírására szolgál a VIATRA lekérdezések nyelve. Ezekből a lekérdezésekből későbbiekben könnyen lehet validációs szabályokat származtatni ([lásd](https://static.incquerylabs.com/projects/viatra/viatra-docs/ViatraDocs.html#_validation)), amelytől most a házi feladat során eltekintünk.

Szintén használható a VIATRA a WebTest nyelv modelljeinek vizsgálatára, számunkra érdekes modellek és modell részletek megtalálására, melyeket aztán fel tudunk használni a programunkban (például kódgenerátorokban).

Végül a modelltranszformációk előfeltételeit is VIATRA lekérdezésekkel tudjuk megadni. (Ez a következő feladatrészben lesz.)

A **webtest.transformation** projekten belül az **queries** könyvtár alatt a **webtest.transformation.queries** csomagból nyissátok meg a **queries.vql** fájlt, és készítsétek el a lentebb definiált jólformáltsági kényszerek ellenőrzésére szolgáló és egyéb lekérdezéseket!

Fontos, hogy a lekérdezések nevei és paraméterei megegyezzenek az alább definiált lekérdezések neveivel és paramétereivel. A lekérdezések változó nehézségűek (1-10 sor közöttiek), és nem nehézségi sorrendben vannak bevezetve.

## Kényszerek
### variableSameElement(var1 : Variable, var2 : Variable)
Két Variable olyan ElementExpression-t tartalmaz, melyek _tag_-je és _label_-je is azonos.

### variableSameString(var1 : Variable, var2 : Variable)
Két Variable olyan StringExpression-t tartalmaz, melyek _value_-ja azonos.

### variableExpressionLoop(e : VariableExpression)
Egy VariableExpression közvetve tartalmazza saját magát egy vagy több Variable-ön (és VariableExpression-ön) keresztül.

### waitSecondsNotInteger(wait : WaitStatement)
WaitStatement _seconds_ property-jének típusa nem Type.INTEGER.

### waitConditionNotBoolean(wait : WaitStatement)
WaitStatement _condition_ property-jének típusa nem Type.BOOLEAN.

## Általános lekérdezések
### twoDeepVariableExpression(e : VariableExpression)
Egy VariableExpression-ben legalább két VariableExpression van egymásba ágyazva.

### tenDeepvariableExpression(e : VariableExpression)
Egy VariableExpression-ben pontosan tíz VariableExpression van egymásba ágyazva.

###  operationWithThreeParameters(o : Operation)
Egy Operation legalább három paramétert tartalmaz.

