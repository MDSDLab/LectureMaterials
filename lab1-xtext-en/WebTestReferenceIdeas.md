# WebTest nyelv referencia - További ötletek

Ebben a listában néhány további hasznos ötlet szerepel, amelyekkel bővíteni lehet a WebTest nyelvet a kényelmesebb használhatóság érdekében.

## Weboldal modellek közötti öröklődés

A WebTest nyelv megkönnyíti egy weboldal vagy akár egy weboldal részeként megjelenő form vagy dialógusablak objektummodelljének elkészítését a **page** kulcsszó segítségével.

Az eredeti WebTest nyelvi leírás szerint weboldal modell szerkezete az alábbi:

```
page <name>
  <variable>...
  <operation>...
end
```

Ebben a kiterjesztésben megengedjük az öröklést is a weboldal modellek között, és csak egy darab őst lehet megadni. Az oldal neve után kettőspontot téve lehet megadni az ős modell nevét:

```
page <name> : <parent:page>
  <variable>...
  <operation>...
end
```

Ha az oldal olyan változót vagy műveletet definiál, amely valamely ősben már szerepelt, akkor az oldal által definiált változó illetve művelet elfedi az ősökben definiáltakat.

Példa:

```
webtest example.WizardTest

page WizardPage
  element next = button "Next"
  element previous = button "Previous"
end

page Name : WizardPage
  element firstName = input "First name..."
  element lastName = input "Last name..."
end

page ContactInfo : WizardPage
  element email = input "E-mail..."
  element phone = input "Phone..."
  element next = button "Next"
end

page Birthday : WizardPage
  element day = input "dd"
  element month = input "mm"
  element year = input "yyyy"
end

page LoginInfo : WizardPage
  element username = input "Username..."
  element password = input "Password..."
  element next = button "Submit" // hides WizardPage.next
end
```

## Aritmetikai műveletek, logikai és egyéb kifejezések

Egy kicsit nehezebb feladat az aritmetikai műveletek (pl. összeadás, kivonás, szorzás, osztás), logikai (pl. egyenlőség, nemegyenlőség, kisebb, nagyobb) és egyéb kifejezések (karakterláncok összefűzése) támogatása. A nehézséget főként az Xtext nyelvtan megalkotása jelenti balrekurzió nélkül.

## Indexelés (...[...])

Ha egy HTML elem egy listát (`<ul>`, `<ol>`) vagy egy táblázatot (`<table>`) reprezentál, szükség lehet egy adott indexű listaelem vagy adott indexű cella elérésére. Ilyenkor használható az index operátor.

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

Példák az indexelés használatára a fenti HTML kódhoz:

```
element greetings = ul "Greetings"
element first = greetings[1]
element second = greetings[2]
assert first contains "Alice"
assert second contains "Bob"

element beverages = ol "Beverages"
assert beverages[1] is "Coffee"
assert beverages[3] is "Milk"

element companies = table "Companies"
assert companies[1,1] is "Company"
assert companies[3,2] is "CEO B"
```

Egy kicsit összetettebb példa:

```
page CompanyOperations
  element edit = button "Edit"
  element delete = button "Delete"
end

context companies[3,3] as CompanyOperations
  click delete
end
```

Indexelésként nem csak számokat, de **string** típusú értékeket is használhatunk. Az indexelés ilyenkor a táblázat fejléce (első sor és első oszlop) alapján történik:

```
page CompanyOperations
  element edit = button "Edit"
  element delete = button "Delete"
end

element companies = table "Companies"
assert companies["Company A","Contact"] is "CEO A"

element bops = companies["Company B","Operations"]
page bops as CompanyOperations
  click edit
end
```

## Kézikönyvek olvashatóságának javítása

Kézikönyvek (**manual**) esetén a kézikönyv tartalma lehetne [CommonMark](https://commonmark.org/) szintaxis, így könnyebb lenne formázni a kimenetet.
