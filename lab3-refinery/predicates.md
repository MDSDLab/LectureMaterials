# Gráfpredikátumok létrehozása a Refinery-ben

A WebTest nyelv metamodellje nem kényszeríti ki a hibamentes modellek létrehozását, ezért jólformáltsági kényszerekre van szükség. Ezen kényszerek ellenőrzéséhez olyan hibamintákat kell megadnunk, amelyek jelzik a hibás modelleket. Ezeket a hibamintákat [Refinery predikátumok](https://refinery.tools/learn/language/predicates/) segítségével tudjuk definiálni. Ezekkel a lekérdezésekkel könnyen definiálhatjuk a [validációs szabályokat](https://refinery.tools/learn/language/predicates/#error-predicates).

A Refinery használható a WebTest nyelv modelljeinek vizsgálatára, érdekes modellek és modell részletek megtalálására is, amelyeket programunkban felhasználhatunk (pl. kódgenerátorok).

## Forrásfájl
Ebben a feladatban a **webtest-refinery/webtest.refinery** fájlon fogtok dolgozni. Ez a fájl több predikátumot tartalmaz, amelyeket implementálnotok kell a WebTest modellek különböző jólformáltsági kényszereinek ellenőrzéséhez, valamint a modellek érdekes részleteinek lekérdezéséhez.

A feladatot tartalmazó _problem_ fájl különböző részekből áll:
- **Metamodell**: ez leírja a WebTest nyelv metamodelljét. *Ezt a részt nem szabad megváltoztatni.*
- **Predikátumok**: itt találhatók a modellre illeszthető predikátumok definíciói. Kezdetben minden predikátumdefiníció _hamis_; a minták soha nem illeszkednek. *Ez az a rész, amelyet a labor során meg kell változtatni.* A megoldások elvárt viselkedése a minták feletti megjegyzésben van leírva.
- **Példánymodell**: Ide írhattok példamodelleket a megoldás teszteléséhez, vagy bemásolhattok egyet az _instance-models_ mappából. A példamodellekben az **Elvárt eredmény** rész leírja, hogy a helyes megvalósításhoz hol várható a minták illeszkedése. A _declare_ rész bevezeti az objektumokat, a pozitív állítások bevezetik a típusokat és az éleket (pl. `Variable(variable1).`), a többi állítás pedig a modell többi értékét hamisra állítja. A modell szándékosan tartalmazhat furcsa részeket, mivel olyan megoldást szeretnénk tesztelni, amely képes felismerni ezeket az értelmetlen részeket.

# A feladatok leírása
Ebben a feladatrészben az alábbi lépéseket kell végrehajtani minden predikátummal:
- Olvassátok el a predikátum felett található specifikációt.
- Módosítsátok a predikátum definícióját (a `<->` után), hogy megvalósítsátok a kívánt specifikációt. Ne változtassátok meg a predikátum nevét vagy a paraméterlistát!
- Ellenőrizzétek a **Table** nézetben a predikátumok illeszkedési halmazát:
    * Ha nincs `error` jelöléssel ellátott sor, akkor egyetértünk a megoldással. (A nem létező `::new` elemek error értéket jelezhetnek, ami elfogadható.)
    * Ellenkező esetben az `error` jelöléssel ellátott sorok a várt viselkedés és a jelenlegi megvalósítás közötti eltérést jelzik. Módosítsátok a predikátum definícióját, amíg nem marad `error` jelöléssel ellátott sor.

> [!IMPORTANT]  
> A példamodellek nem feltétlenül fedik le az összes esetet. Az ti felelősségetek, hogy a predikátumok minden lehetséges modell esetében helyesen működjenek. A megoldás ellenőrzése során további modelleket is fogunk használni.

Új predikátumot lehet bevezetni, de törölni nem. Kérjük, ne módosítsátok a fájl szerkezetét! Ebben a feladatrészben nem szükséges a **GENERATE** funkció használata.

A feladat befejezése után mentsétek el a megoldásotokat (`webtest.refinery` néven), és töltsétek fel GitHubra.

## Implementálandó predikátumok
A következő predikátumokat kell implementálnotok a **webtest-refinery/webtest.refinery** fájlban:

### equalStringExpression(node e1, node e2)
Egyező értékű StringExpression párokat keres. A StringExpression elemeknek különbözőnek kell lenniük.

### equalElementExpression(node e1, node e2)
Két különböző ElementExpression elemet keres, amelyek azonos tag-gel és azonos label-lel rendelkeznek.

### waitSecondsNotInteger(node w)
WaitStatement elemekre illeszkedik, ha azok seconds attribútuma nem INTEGER típusú.

### longWait(node w)
Megkeresi azokat a WaitStatements elemeket, ahol seconds > 10.
Figyeljünk arra, hogy az IntegerExpression egy vagy több VariableExpression-be is beágyazódhat.

### variableExpressionLoop(node e)
A VariableExpression közvetlenül, vagy egy vagy több Variable és VariableExpression-ön keresztül tartalmazza önmagát.

### blockStatementWithThreeStatements(node s)
BlockStatement legalább három utasítást tartalmaz.

### emptyName(node e)
Üres névvel rendelkező elemeket keres. A név üresnek számít, ha üres stringre van állítva.

### emptyVariableExpression(node e)
Keresse meg azokat a VariableExpressions elemeket, amelyek nem mutatnak Variable-re.

### expressionType(node expression, node type)
Rendeljétek hozzá a megfelelő Type-ot minden Expression elemhez az expressionType származtatott értékkel.
