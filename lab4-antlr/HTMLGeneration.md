# HTML kódgenerálás

A [szemantikai elemzés](SemanticAnalysis.md) után biztosak lehetünk benne, hogy a kód helyes, tehát a kódgeneráló visitorban (**main.codegen.WebtestHtmlCodeGenerator**) már csak a kódgenerálás logikájára kell figyelnünk.

A HTML kódgenerálásnál a következőkre kell figyelni:
- az oldal vázát (*html*, *body*, *script* tagek) lehet statikusan generálni, beleértve a beimportált jQuery verziót és a generált JavaScript fájl nevét is (```validation.js```)
- az inputokhoz mindig generálódjon egy megfelelő label is
- a saját hibaüzenetet tartalmazó *div* tag-ek kezdetben legyenek elrejtve
- a HTML attribútum validációk *attribute*-ként jelennek meg a megfelelő *input* mezőkön
- a csoportból egy egyszerű *div* tag-et érdemes generálni

---
**Megjegyzés**

A kódgenerálás során bármilyen technológia használható; a kiinduló projekt [StringTemplate](https://www.stringtemplate.org/) alapú, de tetszőleges kódgeneráló framework, vagy akár natív StringBuilder is használható - ez utóbbi esetben viszont a kódgenerálás valószínűleg sokkal nehezebb lesz.

---

A kódgenerálás során érdemes a példát követni: [példakód](https://github.com/MDSDLab/mdsd-2023-lab4-antlr/blob/main/src/examples/PersonForm.wtiv) --> [generált HTML](https://github.com/MDSDLab/mdsd-2023-lab4-antlr/blob/main/src/examples/generated/PersonForm.html).