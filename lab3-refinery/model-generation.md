# Model generation in Refinery

Az előző feladatrészben gráfpredikátumokat hoztatok létre a WebTest nyelv modelljeinek különböző jólformáltsági kényszereinek ellenőrzéséhez. Ebben a feladatban ezeket a predikátumokat fogjátok használni új [error predikátumokkal](https://refinery.tools/learn/language/predicates/#error-predicates) együtt a Refinery modellgeneráló funkciójának segítségével a WebTest nyelv tesztmodelljeinek generálásához. Egy [példánymodellt](https://refinery.tools/learn/language/logic/) fogtok módosítani, hogy a generátor kiegészíthesse a modellt, és létrehozhasson érvényes teszteseteket, amelyek megfelelnek a predikátumok által megadott jólformáltsági kényszereknek.

## Forrásfájl
Ebben a feladatban a **webtest-refinery/generation.problem** _problem_ fájlon fogtok dolgozni. Ez a fájl a WebTest nyelv példamodelljét tartalmazza. Ezt a modellt fogjátok használni a WebTest nyelvhez tartozó tesztesetek generálásához az előző részben definiált predikátumok segítségével. A feladatot tartalmazó problem fájl különböző részekből áll:
- **Metamodell**: ez a WebTest nyelv metamodelljét írja le. *Ezt a részt nem szabad megváltoztatni.*
- **Predikátumok**: itt megadhatók azok az error predikátumok, amelyek a modellek generálásához használhatók. *Ha a generálási feladatokhoz szükséges, itt új predikátumokat is hozzáadhattok.*
- **Példánymodell**: itt található az Példánymodell definíciója. *Ezt a részt módosítanotok kell a szükséges tesztmodellek generálásához.*

## A feladat leírása

Ebben a feladatban tesztmodelleket fogtok generálni a WebTest nyelvhez. A modellek generálásához felhasználhatjátok az előző feladatban definiált predikátumokat (másoljátok át azt, amelyeiket használni szeretnétek). Új (error) predikátumokat is definiálhattok. Módosítani kell a példánymodellt, hogy a generátor kiegészíthesse a modellt, és olyan érvényes teszteseteket hozzon létre, amelyek megfelelnek a predikátumok által meghatározott jólformáltsági kényszereknek. Hozzáadhattok type scope kényszereket is, hogy korlátozzátok a generált modellek méretét.

Az alábbi követelmények közül kettőhöz definiáltunk error predikátumokat, amelyeket implementálni kell.

Módosítsátok a problem fájlt, hogy a következő jellemzőkkel rendelkező tesztmodelleket tudjatok generálni:

- A kezdeti modell csak a kényszerek kielégítéséhez szükséges helyeken módosítható és bővíthető.
- Minden `BlockStatement` legalább egy `statement` elemet tartalmaz (az `emptyBlockStatement` error predikátumot kell implementálni).
- Biztosítsátok, hogy a generátor bármilyen típusú `Statement` vagy `Expression` elemmel kiegészíthesse azon `BlockStatement` elemeket, amelyek a kezdeti modellben nem tartalmaznak `statement` elemeket. Biztosítsátok, hogy a generátor itt új elemeket is létrehozhasson.
- Minden `IfStatement`-nek tartalmaznia kell egy `then` blokkot (implementáljátok az `emptyIfStatement` error predikátumot).
- Ha egy `IfStatement` nem tartalmaz `then` blokkot a kezdeti modellben, akkor kézzel hozzatok létre egyet, és engedjétek meg a generátornak, hogy bármilyen típusú  `Statement` vagy `Expression` elemmel kiegészítse. Engedjétek meg a generátornak, hogy itt új elemeket hozzon létre.
- Az alábbi, az előző feladatban létrehozott predikátumoknak nem lehetnek illeszkedései a generált modellekben:
    - waitSecondsNotInteger
    - variableExpressionLoop
    - blockStatementWithThreeStatements
- A generátor legalább 1 és legfeljebb 4 új BlockStatement elemet adjon hozzá a modellhez.
- A generált modelleknek összesen legalább 40 és legfeljebb 50 elemük legyen.
- A hozzáadott elemek attribútumértékeit nem kell beállítani.

A feladat befejezése után mentsétek el a megoldását (`generation.problem` néven), és töltsétek fel GitHubra.

Végül legalább 5 modellt generáljatok a **Generate** gombra kattintva. Mentsétek el a generált modelleket a gépetekre (kattints az **Export** gombra a _Graph view_ jobb felső sarkában, válaszd ki a **Refinery** fájltípust, majd kattints a **Download** gombra), és töltsétek fel őket a `generated-models` mappába a repository-ba.