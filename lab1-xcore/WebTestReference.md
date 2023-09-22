# WebTest nyelv referencia

## WebTest fájl szerkezete

A WebTest fájlok **.wt** kiterjesztéssel rendelkeznek. Egy WebTest fájl szerkezete az alábbi:

```
webtest <package>.<class>

(<page> | <test> | <operation>) ...

<statement>...
```

A fájl elején a **webtest** kulcsszó definiálja azt a Java osztályt (&lt;class>) teljes Java package előtaggal (&lt;package>), amely JUnit tesztként a WebTest fájlban leírt teszteket és utasításokat fogja futtatni. Ezt követően tetszőlegesen sok weboldal modell (&lt;page>), teszteset (&lt;test>) és művelet (&lt;operation>) következhet, végül pedig tetszőlegesen sok utasítás (&lt;statement>).

Példa egy ilyen fájlszerkezetre:

```
webtest example.Calculator

page Calculator
  // ...
end

test add3and6
  // ...
end

test multiply23and6
  // ...
end
```

Másik példa egy ilyen fájlszerkezetre:

```
webtest example.Google

operation search using text
  fill textarea "q" with text
  click button "Search"
end

open "https://www.google.com"
search using "jwst"
```

## Weboldal modell (page ... end)

A WebTest nyelv megkönnyíti egy weboldal vagy akár egy weboldal részeként megjelenő form vagy dialógusablak objektummodelljének elkészítését a **page** kulcsszó segítségével.

Egy weboldal modell szerkezete az alábbi:

```
page <name>
  <variable>...
  <operation>...
end
```

A modellnek nevet kell adni (&lt;name>), a modellen belül pedig definiálhatunk változókat (&lt;variable>), amelyek valamilyen HTML taget reprezentálnak, de egyéb (karakterlánc, egész szám, stb.) értékeket reprezentáló változók is megadhatók. Ezen kívül definiálhatunk még műveleteket (&lt;operation>), amelyek valamilyen utasítássorozatot hajtanak végre az elemek segítségével.

### HTML elemet reprezentáló változók

Egy HTML elemet reprezentáló változó definíciójának szerkezete az alábbi:

```
set <name> to <tag> <label>
```

Az elemnek nevet kell adni (&lt;name>), és meg kell határozni, hogy az elem milyen HTML tag-re (&lt;tag>) hivatkozik és milyen címkével (&lt;label>). A tag és címke segít az elem egyértelmű beazonosításában. Az elemek beazonosításnak részleteit a kifejezések szakasz ismerteti.

### operation ... using ... end

Egy művelet szerkezete az alábbi:

```
operation <name> using <parameter>...
  <statement>...
end
```

A műveletnek nevet kell adni (&lt;name>), és opcionálisan lehet tetszőlegesen sok vesszővel elválasztott paramétere (&lt;parameter>). A művelet törzse utasítások sorozata, amelyek felhasználhatják mind az elemeket, mind pedig a művelet paramétereit, illetve hívhatnak más műveleteket.

Példa egy weboldal modellre:

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

## Teszteset (test ... end)

Egy teszteset szerkezete az alábbi:

```
test <name>
  <statement>...
end
```

A tesztesetnek nevet kell adni (&lt;name>). A teszteset törzse utasítások sorozata. A tesztesetek függetlenek egymástól, és tetszőleges sorrendben végrehajthatók.

Példa egy tesztesetre:

```
test login
  fill input "username" with "alice"
  fill input "password" with "secret"
  click button "Sign in"
  assert label "Signed in as: Alice" exists
  assert button "Sign out" exists
end
```

## Utasítások

### set ... to ...

A **set** utasítás egy változót definiál:

```
set <name> to <value>
```

A változó csak egyszer, a definíciója helyén kaphat értéket, ez az érték nem módosítható. A változók típusát nem kell kiírni, a típust a nyelv fordítója automatikusan kikövetkezteti. Egy változó lehetséges típusai:

* **string**: karakterláncot reprezentál
* **integer**: egész számot reprezentál
* **boolean**: logikai értéket reprezentál
* **element**: HTML oldalnak egy elemét (tag-jét) reprezentálja

Példák:

```
set url to "https://www.google.com"
set timeout to 10
set logout to button "Sign out"
set loggedIn to logout exists
```

A fenti példában az egyes változók típusai:

* url: **string**
* timeout: **integer**
* logout: **element**
* loggedIn: **boolean**

### if ... then ... else ... end

Az **if** utasítás feltételes elágazást definiál:

```
if <condition:boolean> then
  <statement>...
else
  <statement>...
end
```

Az **if** kulcsszó után meg kell adni egy feltételt (**boolean** típusú kifejezést), majd a **then** kulcsszó után azokat az utasításokat, amelyeket a feltétel teljesülése esetén kell végrehajtani. Az **else** kulcsszó után megadhatók azok az utasítások, amelyeket akkor kell kiértékelni, ha a feltétel hamis. Az **else** ág opcionális.

Példák:

```
if not loggedIn then
  fill username with "alice"
  fill password with "secret"
  click login
end

if homeButton exists then
  click homeButton
else
  print "Error"
end
```

### while ... do ... end

Az **while** utasítás ciklust definiál:

```
while <condition:boolean> do
  <statement>...
end
```

A **while** kulcsszó után meg kell adni egy feltételt (**boolean** típusú kifejezést), majd a **do** kulcsszó után azokat az utasításokat, amelyeket végre kell hajtani, amíg a feltétel teljesül.

Példa:

```
set nextButton to button "next"
set finishButton to button "finish"

while nextButton exists do
  click nextButton
end
click finishButton
```

### operáció meghívása

Egy operáció meghívásához hivatkozni kell az operáció nevére, majd a **using** kulcsszó után meg kell adni az operációnak átadandó argumentumokat. A hívás két módon történhet: indexelt vagy nevesített paraméterekkel.

Indexelt esetben a paraméterek nevét nem kell kiírni, hanem az argumentumként beadott vesszővel elválasztott értékek a paraméterek definiálási sorrendjében kerülnek átadásra:

```
<operation name> using <value>...
```

Nevesített esetben a vesszővel elválasztott argumentumok név szerint hivatkoznak a paraméterekre, és minden érték az argumentum nevének megfelelő paraméter számára kerül átadásra:

```
<operation name> using <name>:<value> ...
```

Példák:

```
operation login using username,password
  fill input "username" with username
  fill input "password" with password
  click button "Sign in"
end

login using "alice","secret"

login using username:"alice", password:"secret"

login using password:"secret", username:"alice"
```

A fenti három *login* hívás ugyanazt a működést eredményezi.

### open

Az **open** utasítás egy **string** típusú URL-t vár, ezt nyitja meg a böngészőben:

```
open <url:string>
```

Példák:

```
open "https://www.google.com"

set github to "https://github.com"
open github
```

### fill ... with ...

A **fill** utasítás begépel egy karakterláncot a hivatkozott HTML elembe:

```
fill <elem:element> with <text:string>
```

A **fill** utasítás egy **element** típusú elemet (&lt;elem>), valamint egy **string** típusú értéket vár (&lt;text>).

Példák:

```
fill input "username" with "alice"

set password to input "password"
set secret to "secret"
fill password with secret
```

### click ...

A **click** utasítás rákattint a hivatkozott HTML elemre:

```
click <elem:element>
```

Példák:
```
click button "Sign in"

set logout to button "Sign out"
click logout
```

### context ... as ... end

A **context as** utasítás úgy tekinti, hogy a böngészőben megnyitott oldal, vagy a hivatkozott HTML elem tartalma megfelel a hivatkozott weboldal modellnek:

```
context <elem:element> as <page>
  <statement>...
end
```

A megadott &lt;elem> azt jelenti, hogy a **context as** törzsében történő további elemek keresése ezen &lt;elem> HTML elemen belül, vagyis ennek kontextusában értendő. Az &lt;elem> hivatkozás opcionális: amennyiben nincs jelen, akkor az aktuálisan megnyitott teljes weboldalt tekintjük kontextusnak.

A megadott &lt;page> azt jelenti, hogy a **context as** törzsében szereplő utasítások hivatkozhatnak a &lt;page> által definiált weboldal modell elemeire. A &lt;page> által definiált modell elemeit is az &lt;elem> által hivatkozott HTML elem alatt kell keresni. Az **as** &lt;page> hivatkozás is opcionális.

Habár mind az &lt;elem>, mind pedig az **as** &lt;page> hivatkozás opcionális, legalább egyiknek jelen kell lennie.

Példa a teljes oldal modellezésére:

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

open "https://www.calculatorsoup.com/calculators/math/basic.php"
context as Calculator
  wait until display exists
  print "Page opened"
  multiply using "23","6"
  assert display is "138"
  capture display
end
```

Példa egy HTML elem modellezésére, ahol a törlés hatására egy megerősítő dialógusablakot dob fel az oldal (a "Yes" és "No" gombok a "popup" azonosítójú `<div>`-en belül keresendők):

```
page MessageBox
  set yes as button "Yes"
  set no as button "No"
end

click button "Delete"
context div "popup" as MessageBox
  click yes
end
```

Ugzanez a példa egy modell nélküli kontextussal:

```
click button "Delete"
context div "popup"
  click button "Yes"
end
```

### print

A **print** utasítás a paramétereinek értékét kiírja az alkalmazás logjába:

```
print <value>...
```

A bemeneti értékeket vesszővel elválasztva kell megadni.

A **print** utasítás segíthet a teszt folyamatának követésében, de segítséget adhat akár felhasználói útmutató automatikus előállításában is.

Példák:

```
print "Page opened"

set alice to "alice"
set alicePass to "secret"
login using alice, alicePass
print "Logged in as ", alice
```

### assert ...

A **assert** kulcsszó a tesztelést segíti, ellenőrzi, hogy a megadott feltétel teljesül-e:

```
assert <condition:boolean>
```

Amennyiben a feltétel nem teljesül, a JUnit tesztnek Failure-ként kell végződnie.

Példa:

```
test login
  fill input "username" with "alice"
  fill input "password" with "secret"
  click button "Sign in"
  assert label "Signed in as: Alice" exists
  assert button "Sign out" exists
end
```

### wait

A **wait** utasítás olyan esetekben lehet hasznos, amikor a HTML oldal betöltése sok időt vehet igénybe, vagy valamilyen műveletnek az eredményét szeretnénk megvárni.

A **wait ... seconds** utasítás a megadott másodpercig várakozik:

```
wait <time:integer> seconds
```

A **wait until ...** utasítás addig vár, amíg a megadott feltétel nem teljesül:

```
wait until <condition:boolean>
```

Illetve a kettő kombinálható is, ha timeout-tal szeretnénk várni a feltétel teljesülésére:

```
wait <time:integer> seconds until <condition:boolean>
```

Példa 10 másodperces várakozásra:

```
wait 10 seconds
```

A következő példában a login művelet meghívása után megvárjuk, míg a "Sign out" feliratú gomb megjelenik az oldalon, de maximum 5 másodpercig várunk:

```
set timeout to 5
set signout to button "Sign out"
login using "alice","secret"
wait timeout seconds until signout exists
```

## Kifejezések

Változók értékadásakor illetve különböző utasítások argumentumaként átadhatók kifejezések. Ez a szakasz a WebTest nyelvben használható kifejezéseket foglalja össze.

### ... is ...

Az **is** kifejezés ellenőrzi, hogy az adott HTML elem értéke megegyezik-e a jobb oldali értékkel:

```
<elem:element> is <value:string>
```

Az **is** kifejezés eredményének típusa **boolean**.

Az alábbi példa azt ellenőrzi, hogy a bejelentkezés után a weboldal kiírja-e a bejelentkezett felhasználó nevét a megfelelő &lt;div> HTML elembe:

```
login using "alice","secret"
set loggedIn as div "loggedIn"
assert loggedIn is "Logged in as: alice"
```

### ... contains ...

Az **contains** kifejezés ellenőrzi, hogy az adott HTML elem értéke tartalmazza-e a jobb oldali értéket:

```
<elem:element> contains <value:string>
```

A **contains** kifejezés eredményének típusa **boolean**.

Az alábbi példa azt ellenőrzi, hogy a bejelentkezés után a weboldal kiírja-e a bejelentkezett felhasználó nevét a megfelelő &lt;div> HTML elembe:

```
login using "alice","secret"
set loggedIn as div "loggedIn"
assert loggedIn contains "alice"
```

### ... exists

Az **exists** kifejezés ellenőrzi, hogy az adott HTML elem létezik-e:

```
<elem:element> exists
```

Az **exists** kifejezés eredményének típusa **boolean**.

Az alábbi példa azt ellenőrzi, hogy a bejelentkezés után a weboldal megjeleníti-e a kijelentkezés gombot:

```
login using "alice","secret"
set logout as button "Sign out"
assert logout exists
```

### not ...

Az **not** kifejezés tagadja az utána következő boolean kifejezés értékét:

```
not <value:boolean>
```

A **not** kifejezés eredményének típusa **boolean**.

Példa:

```
login using "alice","secret"
set logout as button "Sign out"

if not logout exists then
  print "Sign out is not displayed after signing in."
end
```

### konstans kifejezések

Konstans kifejezésként az alábbiak használhatók:

* karakterlánc (típusa **string**): idézőjelek között a szokásos Java szintaxissal (pl. "Hello World!")
* egész szám (típusa **integer**): a szokásos Java szintaxissal (pl. 10)
* logikai érték (típusa **boolean**): **true** vagy **false**
* HTML elem (típusa **element**): `<tag> <label>`

HTML elem esetén tag (`<tag>`) és címke (`<label>`) segít az elem egyértelmű beazonosításában. A Selenium sokféle [stratégiát](https://www.selenium.dev/documentation/webdriver/elements/locators/) ad egy elem megkeresésére (pl. id attribútum alapján, name attribútum alapján, css osztály alapján, xpath alapján). A WebTest nyelvben ezeket a lehetőségeket elfedjük úgy, hogy a &lt;tag>-&lt;label> értékeket többféle keresési stratégia során is kipróbáljuk, és amelyik stratégia először ad egyértelmű találatot, azt választjuk az adott elem kiválasztására. A kipróbálandó stratégiák és a kipróbálás sorrendje a következő:

* xpath: `.//<tag>[@id='<label>']`
* xpath: `.//<tag>[@name='<label>']`
* xpath: `.//<tag>[@aria-label='<label>']`
* xpath: `.//<tag>[@aria-labelledby='<label>']`
* xpath: `.//<tag>[@label='<label>']`
* xpath: `.//<tag>[@for='<label>']`
* css selector: `#<label> tag`
* xpath: `.//<tag>[@placeholder='<label>']`
* xpath: `.//<tag>[@value='<label>']`
* xpath: `.//<tag>[text()='<label>']`
* xpath: `.//<tag>[contains(text(),'<label>')]`

Ha egyik stratégia sem ad egyértelmű találatot, akkor úgy tekintjük, hogy az elem nem létezik az oldalon.

Példák konstans kifejezésekre:

```
set hello to "Hello World!"
while true do
  wait 10 seconds
  click button "Hello"
end
```

