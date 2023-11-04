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

A kódgenerálás során érdemes a példát követni: [példakód](https://github.com/MDSDLab/mdsd-2023-lab4-antlr/blob/main/src/examples/PersonForm.wtiv) --> [generált JavaScript](https://github.com/MDSDLab/mdsd-2023-lab4-antlr/blob/main/src/examples/generated/validation.js).