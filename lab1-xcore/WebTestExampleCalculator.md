# Példa: számológép

Mivel egy weboldal elemeinek megkeresése komplex Selenium kifejezésekből áll, és egy elemre többször is szükség lehet a futtatás során, a Selenium fejlesztői azt javasolják, hogy építsünk egy objektummodellt, amely a vezérelni kívánt weboldalt reprezentálja, és elrejti az egyes elemek megtalálásának részleteit. Ennek köszönhetően a teszteket egy magasabb absztrakciós szinten, a weboldal fogalmainak segítségével írhatjuk le.

A hagyományos Selenium tesztelésnél ezt az objektummodellt elkészíthetjük Java nyelven. A WebTest nyelv azonban a weboldal objektummodelljének elkészítését is megkönnyíti a **page** kulcsszó segítségével.

Példaként tekintsük az alábbi számológépet működtető weboldalt: [CalculatorSoup](https://www.calculatorsoup.com/calculators/math/basic.php)

A WebTest nyelven az alábbi modellt készíthetjük hozzá:

```
page Calculator
  set display to input "number display"
  set clear to button "AC"
  set add to button "+"
  set subtract to button "-"
  set multiply to button "×"
  set divide to button "/"
  set compute to button "="
  
  operation binaryOperation using left,op,right
    click clear
    fill display with left
    click op
    fill display with right
    click compute
  end
  
  operation multiply using left,right
    binaryOperation using left,multiply,right
  end
end
```

A fenti példában a **page** kulcsszóval egy *Calculator* nevű oldalt írunk le, amelybe felvesszük a számunkra szükséges elemeket reprezentáló változókat a **set** kulcsszóval:

* display: a számológép kijelzője, ebbe lehet begépelni egy számot
* clear: a kijelzőt törlő gomb
* add: az összeadás művelet gombja
* subtract: a kivonás művelet gombja
* multiply: a szorzás művelet gombja
* divide: az osztás művelet gombja
* compute: a végeredmény számítását végző gomb

Ezután az **operation** kulcsszóval definiálunk egy *binaryOperation* általános bináris műveletet, amely három paraméterrel rendelkezik:

* left: a bal oldali operandus értéke
* op: a végrehajtandó művelet gombja
* right: a jobb oldali operandus értéke

A *binaryOperation* művelet először megnyomja az *AC* feliratú gombot, hogy törölje a kijelző értékét, majd beírja a *left* paraméter értékét a kijelzőbe, megnyomja az *op* által reprezentált gombot, beírja a *right* paraméter értékét a kijelzőbe, és végül megnyomja a kiértékelés gombot.

A *binaryOperation* segítségével egy másik műveletet is definiálunk *multiply* néven, amely a *left* és a *right* paraméter értékét összeszorozza úgy, hogy a *binaryOperation* művelet *op* paramétereként a szorzás gombot adja át.

A fenti modellt a következőképpen használhatjuk a WebTest nyelvben:

```
open "https://www.calculatorsoup.com/calculators/math/basic.php"
context as Calculator
  wait until display exists
  print "Page opened"
  multiply using "23","6"
  assert display is "138"
  capture display
end
```

Ez a kód az **open** kulcsszóval megnyitja a weboldalt, majd a **context as** segítségével úgy tekinti, hogy az oldal megfelel a fent definiált *Calculator* objektummodellnek, vagyis feltételezi, hogy a kijelző és a gombok mind megtalálhatók rajta. A **wait** parancs addig várakozik, amíg a *display* elem meg nem jelenik. A **print** kulcsszó kiírja az alkalmazás logba, hogy *Page opened*, vagyis hogy az oldal betöltődött. Majd a *multiply* művelet meghívása következik *23* és *6* értékekkel. Az **assert** kulcsszó ellenőrzi, hogy az eredmény a kijelzőn az elvárt *138* értéket tartalmazza-e. Végül a **capture** kulcsszó úgy gördíti az oldalt, hogy a kijelző látható legyen, majd élénk színű kerettel kiemeli azt, egy képernyőképet készít az oldal tartalmáról, majd megszünteti a kiemelést.
