# Bővítmények leírása

A feladathoz 3 bővítmény tartozik, melyek közül minden [csapat](ExtensionsTable.md) egy darabot kap. A másik két bővítmény opcionálisan megoldható, de pluszpont nem jár érte. 

## Validator 

Az alap feladat szerint bármely validátor alkalmazható bármely input esetén, ami azonban nem mindig eredményezne érvényes kombinációkat. Egészítsük ki a szerkesztőt, hogy figyelmeztetéssel jelezze az érvénytelen kombinációkat:

 * CheckBox, ill. Number esetén nem alkalmazható MaxLength, ill. Pattern
 * Vezessen be egy új validátort, ami csak Password típusú elemeknél alkalmazható és amivel megadható azok minimum hossza!

A beviteli mezők esetén is ellenőrizni kell bizonyos szabályokat:

 * A Text és HeaderText elemek szöveg mezőjében csak alfanumerikus karaktereket engedélyezzük, a többit automatikusan vegyük ki a mezőből a szerkesztés végén (pl. &lt;br&gt; --> br)!
 
## Parent elements

Az egyes elemek bizonyos megkötések mentén tetszőlegesen ágyazhatóak egymásba. A következő esetekben viszont ez hibás működéshez vezethet, ezért a szerkesztőt ki kell egészíteni:

* A beviteli mezőknél elvárható, hogy azok mindig egy Form-on belül legyenek! Vezessen be egy figyelmeztetést az Input mezőnél, ami ellenőrzi, hogy a mezőt direkt, vagy indirekt módon tartalmazza-e egy Form (tehát nem elegendő a közvetlen tartalmazót megvizsgálni)!
* Ne engedje meg, hogy Cell elem Row-n kívül, ill. Row elem Table-ön kívül szerepeljen! (azaz pl. közvetlenül egy Formban)
* Div esetén adjon figyelmeztetést, ha nincs benne egyetlen elem sem és a magasság/szélesség sincs beállítva! (azaz nem placeholderként használjuk)

## Table formatter

A táblázatok esetében vizuálisan zavaró, ha az egy oszlopba tartozó cellák szélessége eltér egymástól az egyes sorokban. Tehát pl. a harmadik oszlop celláitól elvárható, hogy azonos szélességűek legyenek függetlenül attól, hogy hányadik sorról van szó.

* Táblázat esetén adjon figyelmeztetést, ha eltérő cellaszélesség van megadva az egyes sorokban a cellákhoz!
* Tegye lehetővé, hogy az oszlopoknál meg lehessen adni azok szélességét. Ha meg van adva, akkor a celláknál ne legyen mód ennek a felülírására! (Elrejtheti, vagy letilthatja az opciót, mindkét megoldás megfelelő, de csak ha meg van adva az oszlopnál a szélesség!)

Egészítse ki továbbá a táblázat elemet, hogy megadható legyen a keret szélessége (Size-ként) és színe (a szín legyen vizuálisan kiválasztható, ne kóddal kelljen megadni)!
