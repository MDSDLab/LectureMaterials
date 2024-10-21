# JavaScript kódgenerálás

A JavaScript kódgenerálásnál a következőkre kell figyelni:
- a [jQuery](https://jquery.com/) használata erősen ajánlott
- a *submit* gomb megnyomásakor (nem a *submit* eseménykezelőjében!) érdemes minden hibaüzenetet alapállapotba állítani (elrejteni)
- a HTML attribútum validációk előbb fognak "lefutni", mint a saját (összehasonlításos, vagy bővítményben szereplő) validációk
    - előbbit az *input* mezők *invalid* eseménykezelőjébe, utóbbit a *submit* eseménykezelőjébe érdemes rakni
- a HTML attribútum validációt a [ValidityState](https://developer.mozilla.org/en-US/docs/Web/API/ValidityState) segítségével érdemes megoldani
- láthatóság szabályozásnál az inverz működésre se felejtsünk el kódot generálni
- összehasonlításos validációnál a referált *input* mező értékére (jQuery selector esetén ```.val()```) hivatkozzunk a generált kódban

---
**Megjegyzés**

A kódgenerálás során bármilyen technológia használható; a kiinduló projekt [StringTemplate](https://www.stringtemplate.org/) alapú, de tetszőleges kódgeneráló framework, vagy akár natív StringBuilder is használható - ez utóbbi esetben viszont a kódgenerálás valószínűleg sokkal nehezebb lesz.

---

A kódgenerálás során érdemes a példát követni: .

Néhány tipp a kódgenerátor elkészítéséhez (elsősorban StringTemplate használata esetén):
- tanulmányozzuk a **WebtestInputCodeGenerator** osztályt
    - a generált kód a *generatedCode* tagváltozóba kerül, a **WebtestInputRunner** ezt használja, ezen nem kell változtatnunk
    - itt található példa a StringTemplate egyszerű használatára is
- először generáljuk le a főbb eseménykezelők (pl. submit, submit click) vázát a **JsGen** fájl **js** template-jének kiegészítésével
    - később ezt a template-et ki kell egészíteni paraméterekkel is
- érdemes több template-et is definiálni a **JsGen** fájlban
    - pl. egy HTML attribútum validációs template fejléce: ```htmlAttributeValidityCheck(id, errorDisplay, validityAttribute)```
- a **WebtestJsCodeGenerator** osztályban visit függvényeket kell felüldefiniálni, a korábbiakhoz hasonló módon
    - itt használjuk az elkészített template-eket is, a **WebtestInputCodeGenerator** osztályban látottakhoz hasonlóan (további részletekért ld. a StringTemplate dokumentációt)
- a kódgenerálás során érdemes a kiadott mintapéldát követni: [példakód](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/PersonForm.wtiv) --> [generált JavaScript](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/generated/validation.js)