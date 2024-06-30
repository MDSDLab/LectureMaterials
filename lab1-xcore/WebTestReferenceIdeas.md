# WebTest nyelv referencia - További ötletek

Ebben a listában néhány további hasznos ötlet szerepel, amelyekkel bővíteni lehet a WebTest nyelvet a kényelmesebb használhatóság érdekében.

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
