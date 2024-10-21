# WebTestInputValidation nyelv specifikáció

Az alábbiakban a nyelv céljának ismertetése után a nyelvi elemei kerülnek felsorolásra és rövid bemutatásra. A felsorolás a [bővítményeket](Extensions.md) nem, csak a közös, minden csapat által megvalósítandó nyelvi elemeket tartalmazza. Példák az itt található nyelvi elemekre a bemeneti [példakódban](https://github.com/MDSDLab/mdsd-2024-lab4-antlr/blob/main/src/examples/PersonForm.wtiv) találhatók.

## Áttekintés
A nyelv célja, hogy a WebTest nyelv számára szolgáltasson bemeneti fájlként HTML és JavaScript fájlokat, melyek kizárólag HTML **formok** validációját írják le. A nyelv HTML **input** tageket ír le, a struktúra mellett megadva az elemek validációját is. A struktúra leírásból HTML-t, a validációs kódból pedig JavaScript kódot tudunk generálni. A validáció során lehetőségünk van beépített HTML validációt használni, más **input** elemek értéke alapján validálni, vagy azok láthatóságát szabályozni.

## Form definíció
A nyelv mindig pontosan egy form definícióját tartalmazza, megadva annak id-ját.

```
form <id>

...

end
```

## Input definíció
A formon belül tetszőleges számú input mezőt tudunk definiálni.

### Struktúra
Input mező definiálásakor opcionálisan megadhatjuk, hogy el legyen-e rejtve (**hidden**) az oldal betöltésekor. Ezen kívül meg kell adnunk az input [típusát](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input), id-ját, és nevét. Típusból jelenleg csak a következőket támogatja a nyelv: **text**, **number**, **date**, **checkbox**. Ezután a validációs blokk következik.

```
<hidden> input <type> <id> <name>
{
    <validation>
}
```

### HTML attribútum validáció
A nyelv támogatja a beépített HTML input validációs [attribútumokat](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#attributes). Ezek közül csak a következőket támogatjuk: **minlength**, **maxlength**, **min**, **max**, **step**, **pattern**, **required**. A **required** kivételével mindnek paramétere is kell, hogy legyen. Mivel a nyelv célja, hogy automatikusan generált tesztesetek számára szolgáltasson bemenetet, ezért a beépített HTML validációs visszajelzéseket (a kódgenerálás során) ki kell majd kapcsolnunk, helyette saját hibaüzeneteket adunk meg. Ezt a "=>" szintaxissal tehetjük meg.

```
<attribute validation> <parameter> => <error message>
```

---
**Megjegyzés**

Mivel a böngésző **maxlength** attribútum beállítása esetén automatikusan lekorlátozza az adott szövegdoboz méretét, ezért itt nem lehetséges a saját hibaüzeneteket JavaScript segítségével megjelenítenünk, mivel a hibát (túlcsordulást) nem tudjuk előidézni. Ettől függetlenül a teljesség kedvéért szerepel a **minlength** mellett a **maxlength** is a nyelvben.

---

### Összehasonlításos validáció
A nyelv lehetőséget biztosít egyszerű összehasonlításra is más input mezők értékével: **eq** (egyenlő), **neq** (nem egyenlő), **lt** (kisebb), **gt** (nagyobb). Ezeknek mind pontosan egy paramétere van: egy másik input mező id-ja.

```
<comparative validation>(<input id>)
```

### Láthatóság szabályozása
Validációs szabályok megadásán kívül, **checkbox** típusú input mezőnél lehetőségünk van egy másik input mező vagy csoport megjelenítésére (**show**) vagy elrejtésére (**hide**). Ezt az **on?** és **off?** szintaxissal tehetjük meg, ami a checkbox bepipált (checked) vagy nem bepipált állapotváltozására utal. Támogatnunk kell az inverz működést is, tehát ha például egy checkbox bepipálására egy másik mező megjelenik, akkor a checkbox kipipálására ennek a mezőnek el kell tűnnie. Egy **checkbox** input mezőnek akárhány láthatósági szabálya lehet.

```
<on | off>? <show | hide> <id>
```

## Csoport definíció
Input mezők mellett egy form tartalmazhat csoportokat is. Egy csoport tetszőleges számú inputot tartalmazhat, más csoportot nem. A csoportnak is van id-ja. az input mezők és csoportok között globálisan egyedi minden id. Csoport esetén is opcionálisan megadható a **hidden** állapot.

```
<hidden> group <id>
{
    <inputs>
}
```