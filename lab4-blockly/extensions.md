# Bővítmények leírása

A feladathoz 4 bővítmény tartozik, melyek közül minden [csapat](ExtensionsTable.md) egy darabot kap. A másik két bővítmény opcionálisan megoldható, de pluszpont nem jár érte. 

## Input validator 

Az alap feladat szerint bármely validátor alkalmazható bármely input esetén, ami azonban nem mindig eredményezne érvényes kombinációkat. Egészítsük ki a szerkesztőt, hogy figyelmeztetéssel jelezze az érvénytelen kombinációkat:

 * CheckBox esetén nem alkalmazható MinLength, ill. Pattern
 * Number esetén nem alkalmazható MinLength, ill. Pattern

 Vezessen be továbbá egy új validátort, ami csak Number típusú elemeknél alkalmazható és amivel megadható azok minimum és maximum értéke!

## Parent elements

Az egyes elemek bizonyos megkötések mentén tetszőlegesen ágyazhatóak egymásba. A következő esetekben viszont ez hibás működéshez vezethet, ezért a szerkesztőt ki kell egészíteni:

* A beviteli mezőknél elvárható, hogy azok mindig egy Form-on belül legyenek! Vezessen be egy figyelmeztetést az Input mezőnél, ami ellenőrzi, hogy a mezőt direkt, vagy indirekt módon tartalmazza-e egy Form (tehát nem elegendő a közvetlen tartalmazót megvizsgálni)!
* Ellenőrizze, hogy a ListItem elemek egy List elemen belül vannak-e! (szintén lehetséges, hogy közvetett a tartalmazás)
* TextBlock esetén adjon figyelmeztetést, ha nincs benne egyetlen elem sem! (a Children üres)

## CSS style

Jelenleg lehetőség van hivatkozni CSS osztályra bizonyos pontokon az elemekhez, hogy azok kinézetét testreszabjuk. 
Hozzon létre egy új elemet CssStyle néven és alakítsa át a CSS elemet, hogy tartalmazhasson ilyen CssStyle típusú elemeket (CHILDREN)!

A CssStyle elem speciális: legördülő listából (TYPE) választhassuk ki, hogy mit szeretnénk testreszabni, majd a választástól függően legyen lehetőség megadni az adott tulajdonság értékét (VALUE)! 
Tehát egy olyan elemre van szükség, ahol a legördülő menüből történő érték választástól függően más és más beviteli mező jelenik meg az adott elemen. 

 * Font: szöveges formába megadható a font
 * Background-color: színként megadható a háttér szín
 * Padding: megadható a margó mérete egyetlen szám formájában
 * Font-Size: számként megadható a betűméret

## Safe input

Bár egy ilyen környezet kialakításakor a biztonsági aspektusok ritkán kerülnek előtérbe, néhány támadási mechanizmust mégis érdemes már ezen a szinten kivédeni. Egészítsük ki a szerkesztőt a következő mechanizmusokkal:

 * A szerkesztési térben alapesetben jelenjen meg egy WebPage, amit ne lehessen se mozgati se letörölni!
 * A Text elem szöveg mezőjében csak alfanumerikus karaktereket engedélyezzük, a többit automatikusan vegyük ki a mezőből a szerkesztés végén (pl. &lt;br&gt; --> br)!
 * A Link esetén ellenőrizze a JavaScriptes URL osztály segítségével, hogy tényleg weboldalt adtak-e meg (és törölje a megadott értéket, ha az nem weboldal)!
