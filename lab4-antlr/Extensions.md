# Bővítmények leírása

A feladathoz három bővítmény tartozik, melyek közül minden [csapat](ExtensionsTable.md) egy darabot kap. A másik két bővítmény opcionálisan megoldható, de pluszpont nem jár érte. A táblázatban a "Hibaüzenet" az első, a "JavaScript" a második, a "Konkatenáció" pedig a harmadik bővítményre utal.

## Saját hibaüzenet kiegészítése

Tegyük rugalmasabbá a saját hibaüzenetek kezelését: **%id%**, **%name%**, **%type%**-al hivatkozhassunk az input mező megfelelő tulajdonságaira (id, type, name), a JavaScript kódban ezeknek kell megjelennie. A mező értékére is hivatkozhassunk a **%value%** szintaxissal, ezt külön kell kezelni a kódgenerálás során (jQuery selector esetén ```.val()```).

Példák (generált kód [itt](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/generated/validation.js)):
```
required => "Input %id% '%name%' of type %type% is mandatory!"
```

```
pattern("[A-Z][a-z]*") => "Last name (%value%) must begin with a capital letter!"
```

## Natív JavaScript validáció támogatása

Vezessünk be egy újabb validációs formát: legyen lehetőség natív JavaScript kódot futtatni. Egy JS blokkon belül, 1-1 utasítás (kifejezés) állhat, saját hibaüzenettel, pontosvesszővel elválasztva. Az utasításokat nem kell elemezni, nyers szövegként tekinthetők (TIPP: a nyelvtanban is használjunk nagyon általános leírást). Kódgenerálásnál az input mező értékére futtassuk az egyes utasításokat, és írjuk ki a hibaüzenetet, ha false-al tér vissza a kód.

Példa (generált kód [itt](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/generated/validation.js)):
```
JS
{
    startsWith("A"); => "First name must start with 'A'!"
    substring(1, 4) == "abc"; => "First name must contain 'abc' after the first character!"
}
```

---
**Megjegyzés**

A biztonsági kockázatokkal sem kell foglalkozni, semmilyen ellenőrzést nem kell végezni a JS blokkon.

---

## String konkatenáció bevezetése

Egészítsük ki az összehasonlításos validációt: egyszerű id paraméter mellett engedjük meg a konkatenáció műveletét is, amely id és string literal kifejezésekre támogatott. A generált kódban a konkatenációból mindig string generálódjon. Ez a JavaScript sajátosságai miatt egyszerűen megoldható.

Példa (generált kód [itt](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/generated/validation.js)):
```
eq(lname + "ov") => "First name must be last name + ov!"
```