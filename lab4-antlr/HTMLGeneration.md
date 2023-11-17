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

Néhány tipp a kódgenerátor elkészítéséhez (elsősorban StringTemplate használata esetén):
- tanulmányozzuk a **WebtestInputCodeGenerator** osztályt
    - a generált kód a *generatedCode* tagváltozóba kerül, a **WebtestInputRunner** ezt használja, ezen nem kell változtatnunk
    - itt található példa a StringTemplate egyszerű használatára is
- először generáljuk le az oldal statikus vázát a **HtmlGen** fájl **html** template-jének kiegészítésével
    - később ezt a template-et ki kell egészíteni paraméterekkel is
- érdemes több template-et is definiálni a **HtmlGen** fájlban
    - pl. egy input generáló template fejléce: ```input(type, id, name, attributes, isHidden)```
- a **WebtestHtmlCodeGenerator** osztályban visit függvényeket kell felüldefiniálni, a korábbiakhoz hasonló módon
    - itt használjuk az elkészített template-eket is, a **WebtestInputCodeGenerator** osztályban látottakhoz hasonlóan (további részletekért ld. a StringTemplate dokumentációt)
- a kódgenerálás során érdemes a kiadott mintapéldát követni: [példakód](https://github.com/MDSDLab/mdsd-2023-lab4-antlr/blob/main/src/examples/PersonForm.wtiv) --> [generált HTML](https://github.com/MDSDLab/mdsd-2023-lab4-antlr/blob/main/src/examples/generated/PersonForm.html)