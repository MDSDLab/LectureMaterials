# A mögöttes adatstruktúra felépítése

A [nyelvtan](Grammar.md) elkészítése után a következő lépés egy olyan adatstruktúra felépítése, amely később a szemantikai elemzés és kódgenerálás során is felhasználható. Ez hasonló egy klasszikus szimbólumtábla felépítéséhez, de esetünkben egyszerűbb annál. A felépítéshez célszerű a **main.WebtestInputTableBuilder** osztályt (a visitor mintát alkalmazva), a tábla karbantartásához pedig a **data.WebtestInputTable** osztályt használni. A klasszikus szimbólumtábla építéshez hasonlóan már itt érdemes a hibakezeléssel foglalkozni, mivel ez megkönnyíti a dolgunkat a szemantikai elemzésnél.

Az input mezők adatainak tárolására a **data.WebtestInput** osztály használható, de létre lehet hozni egyéb segédosztályokat is. Érdemes elgondolkodni a csoportok struktúrájának tárolásán is.

Mivel erre a lépésre a későbbi feladatok erősen építenek, a megoldás lépéseit vázlatosan is megadjuk:
- a **WebtestInput** osztály kiegészítése a megfelelő tagváltozókkal
    - gondoljuk meg, hogy mit szeretnénk az *input* elemekről tárolni, valamint, hogy hogyan kezeljük a *group* elemeket
- a **WebtestInputTable** osztály kiegészítése, hogy képes legyen **WebtestInput**-okat tárolni
    - *lookup* és *insert* műveletekre biztosan szükség lesz, de egyéb műveleteket is tehetünk bele
- a **WebtestInputTableBuilder** osztály kiegészítése a megfelelő *visit* metódusokkal, melyek felépítik a táblát (**WebtestInputTable** tagváltozót)