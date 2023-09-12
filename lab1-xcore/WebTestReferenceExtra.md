# WebTest nyelv referencia - Bővítmények

## BasePages bővítmény

A WebTest nyelv megkönnyíti egy weboldal vagy akár egy weboldal részeként megjelenő form vagy dialógusablak objektummodelljének elkészítését a **page** kulcsszó segítségével.

Az eredeti WebTest nyelvi leírás szerint weboldal modell szerkezete az alábbi:

```
page <name>
  <variable>...
  <operation>...
end
```

Ebben a kiterjesztésben megengedjük az öröklést is a weboldal modellek között. Az oldal neve után kettőspontot téve vesszőkkel elválasztva lehet megadni az ős modellek neveit:

```
page <name> : <parent:page>...
  <variable>...
  <operation>...
end
```

Ha az oldal olyan változót vagy műveletet definiál, amely valamely ősben már szerepelt, akkor az oldal által definiált változó illetve művelet elfedi az ősben definiáltakat. Ha különböző ősökben szereplő változók illetve műveletek ütköznek egymással, akkor a fordítónak hibát kell jeleznie, és a hiba feloldható úgy, hogy a leszármazott a saját definíciójával elfedi az ősökben előforduló ütközéseket.

Példa:

```
webtest example.WizardTest

page WizardPage
  set next to button "Next"
  set previous to button "Previous"
end

page Name : WizardPage
  set firstName to input "First name..."
  set lastName to input "Last name..."
end

page ContactInfo : WizardPage
  set email to input "E-mail..."
  set phone to input "Phone..."
  set next to button "Next"
end

page Birthday : WizardPage
  set day to input "dd"
  set month to input "mm"
  set year to input "yyyy"
end

page LoginInfo : WizardPage
  set username to input "Username..."
  set password to input "Password..."
  set next to button "Submit" // hides WizardPage.next
end
```

## Capture bővítmény

A **capture** kulcsszó képernyőképet készít a böngészőben látható HTML oldalról:

```
capture page
```

A **capture** kulcsszónak egy HTML elemet is lehet adni paraméterként:

```
capture <elem:element>
```

Ekkor a **capture** kulcsszó úgy gördíti az oldalt, hogy a hivatkozott HTML elem látható legyen, majd élénk színű kerettel kiemeli azt. Ezután képernyőképet készít a böngészőben látható HTML oldalról, végül megszünteti a kiemelést.

A kiemeléshez és görgetéshez az alábbi JavaScript kód használható, ahol `arguments[0]` a kiemelendő elemet jelenti:

```javascript
arguments[0].style.outline = 'red solid 4px'; arguments[0].style.outlineOffset = '-4px';
arguments[0].scrollIntoView(true);
```

A kiemelés megszüntetéséhez az alábbi JavaScript kód használható, ahol `arguments[0]` a kiemelendő elemet jelenti:

```javascript
arguments[0].style.outline = ''; arguments[0].style.outlineOffset = '';
```

A **capture** az elkészült képet PNG fájlként lementi, és az alkalmazáslogba kiírja, hogy `Captured: <filename.png>`. A fájlnév generálásánál fontos, hogy a fájl neve egyedi legyen (pl. sorfolytonos számozás egy statikus változóban), és különböző **capture** utasítások ne írják felül egymás képeit.

A **capture** utasítás felhasználói útmutató automatikus előállításában is segíthet.

Példa:

```
print "A bejelentkezéshez nyomjuk meg a Sign in gombot:"
capture button "Sign in"
click button "Sign in"
```

## ForEach bővítmény

Ha egy HTML elem nem egyértelműen azonosítható be a `<tag> <label>` kombinációval, szükség lehet az összes lehetséges elem bejárására (pl. lista minden elemének vagy a táblázat minden sorának bejárása). Erre szolgál a **foreach** ciklus:

```
foreach <item:element> in <list:element>
  <statement>...
end
```

Vegyük az alábbi HTML kódot:

```html
<ul id="Greetings">
  <li>Hello, Alice</li>
  <li>Hello, Bob</li>
</ul>

<ol id="Beverages">
  <li>Coffee</li>
  <li>Tea</li>
  <li>Milk</li>
</ol> 

<table id="Companies">
  <tr>
    <th>Company</th>
    <th>Contact</th>
    <th>Operations</th>
  </tr>
  <tr>
    <td>Company A</td>
    <td>CEO A</td>
    <td><input type="button" value="Edit"/><input type="button" value="Delete"/></td>
  </tr>
  <tr>
    <td>Company B</td>
    <td>CEO B</td>
    <td><input type="button" value="Edit"/><input type="button" value="Delete"/></td>
  </tr>
</table> 
```

Példák a **foreach** használatára a fenti HTML kódhoz:

```
context ul "Greetings"
  set greetings to li ""
  foreach message in greetings
    assert message contains "Hello"
  end
end

context ol "Beverages"
  foreach beverage in li ""
    print beverage
  end
end

context table "Companies"
  foreach company in tr ""
    click button "Edit"
  end
end
```

Egy kicsit összetettebb példa, ahol a táblázat egy sorát egy modellel reprezentáljuk:

```
page CompanyOperations
  set edit to button "Edit"
  set delete to button "Delete"
end

context table "Companies"
  foreach company in tr ""
    context company as CompanyOperations
      click delete
    end
  end
end
```

## JavaScript bővítmény

A **javascript** kulcsszó lefuttat egy adott JavaScript kódot az oldalon:

```
javascript <code:string>
```

A **javascript** kulcsszónak argumentumokat is lehet adni vesszőkkel elválasztva:

```
javascript <code:string> using <value>...
```

A **javascript** kulcsszó lefuttatja az adott JavaScript kódot a megadott argumentumokkal.

Példa, amely megjelenít egy dialógus ablakot:

```
javascript 'alert("Hello World!");'
```

Példa, amely beállítja a keresőmező értékét a keresendő szövegre:

```
set searchField to input "q"
javascript 'arguments[0].value=arguments[1];' using searchField, "jwst"
```

Példa, amely úgy görgeti az ablakot, hogy a keresőgomb látható legyen:

```
javascript 'arguments[0].scrollIntoView(true);' using button "Search"
```

## Manual bővítmény

Egy kézikönyv szerkezete az alábbi:

```
manual <name>
  <statement>...
end
```

A kézikönyvnek nevet kell adni (&lt;name>). A kézikönyv törzse utasítások sorozata. A kézikönyvek függetlenek egymástól, és tetszőleges sorrendben végrehajthatók. A kézikönyvek kimenetét (**print** ill. **capture** utasítások eredménye) egy HTML dokumentumba kell írni, amely szöveges kézikönyvként szolgál a tesztelt weboldal használatához.

Példa egy kézikönyvre:

```
manual LoginLogout
  print "<h1>Bejelentkezés-kijelentkezés</h1>"
  print "<p>Az alkalmazáshoz való bejelentkezéshez írjuk be a felhasználónevet:</p>"
  fill input "username" with "alice"
  capture input "username"
  print "<p>Majd a jelszót:</p>"
  fill input "password" with "secret"
  capture input "password"
  print "<p>Végül pedig nyomjuk meg a <b>Sign in</b> feliratú gombot:</p>"
  capture button "Sing in"
  click button "Sing in"
  print "<p>A kijelentkezéshez a <b>Sign out</b> feliratú gomb használható:</p>"
  capture button "Sing out"
  click button "Sing out"
end
```

Ezt a példát futtatva az alábbi HTML kódnak áll elő **LoginLogout.html** néven:

```html
<html>
    <body>
        <h1>Bejelentkezés-kijelentkezés</h1>
        <p>Az alkalmazáshoz való bejelentkezéshez írjuk be a felhasználónevet:</p>
        <img src="screenshot001.png" /> 
        <p>Majd a jelszót:</p>
        <img src="screenshot002.png" /> 
        <p>Végül pedig nyomjuk meg a <b>Sign in</b> feliratú gombot:</p>
        <img src="screenshot003.png" /> 
        <p>A kijelentkezéshez a <b>Sign out</b> feliratú gomb használható:</p>
        <img src="screenshot004.png" /> 
    </body>
</html>
```

A keletkező HTML szebbé tehető CSS stílusokkal (pl. bootstrap).

Egy WebTest fájl szerkezete kézikönyvekkel bővítve az alábbi:

```
webtest <package>.<class>

(<page> | <test> | <manual> | <operation>) ...

<statement>...
```

A fájl elején a **webtest** kulcsszó definiálja azt a Java osztályt (&lt;class>) teljes Java package előtaggal (&lt;package>), amely JUnit tesztként a WebTest fájlban leírt teszteket és utasításokat fogja futtatni. Ezt követően tetszőlegesen sok weboldal modell (&lt;page>), teszteset (&lt;test>), kézikönyv (&lt;manual>) és művelet (&lt;operation>) következhet, végül pedig tetszőlegesen sok utasítás (&lt;statement>).


## TestParams bővítmény

Egy paraméterezett teszteset szerkezete az alábbi:

```
test <name> using <parameter>... 
  with <value>...
  <statement>...
end
```

A **using** kulcsszó után meg kell adni a paramétereket vesszővel elválasztva, majd a **with** kulcsszó után meg kell adni az argumentumokat. A **with** kulcsszó többször is használható: minden egyes használat egy-egy új példányt jelent a tesztből. A paraméterezett tesztekből [JUnit 5 paraméterezett teszteket](https://www.baeldung.com/parameterized-tests-junit-5) kell előállítani.

Az argumentumok megadása két módon történhet: indexelt vagy nevesített paraméterekkel.

Indexelt esetben a paraméterek nevét nem kell kiírni, hanem az argumentumként beadott vesszővel elválasztott értékek a paraméterek definiálási sorrendjében kerülnek átadásra:

```
test login using username,password 
with "Alice","secretA"
with "Bob","secretB"
  fill input "username" with username
  fill input "password" with password
  click button "Sign in"
  assert label "Signed in as:" contains username
  assert button "Sign out" exists
end
```

Nevesített esetben a vesszővel elválasztott argumentumok név szerint hivatkoznak a paraméterekre, és minden érték az argumentum nevének megfelelő paraméter számára kerül átadásra:

```
test login using username,password 
with username:"Alice",password:"secretA"
with password:"secretB",username:"Bob"
  fill input "username" with username
  fill input "password" with password
  click button "Sign in"
  assert label "Signed in as:" contains username
  assert button "Sign out" exists
end
```
