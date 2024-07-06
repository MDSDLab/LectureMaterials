# Nyelvtan elkészítése

## Feladat

A WebTest nyelv szöveges reprezentációjának feldolgozásához szükség van egy fordítóra. A fordító előállításához meg kell adni a szöveges reprezentáció formális leírását. Erre szolgál az Xtext nyelvtan. Az Xtext szintaxisról részletes leírás [itt található](https://eclipse.dev/Xtext/documentation/301_grammarlanguage.html).

A **webtest.dsl** projekten belül az **src** könyvtár alatt a **webtest.dsl** csomagból nyissátok meg a **WebTestDsl.xtext** fájlt, és készítsétek el a WebTest nyelv nyelvtanát!

Fontos, hogy nem az Xcore metamodellt kell mechanikusan leképezni nyelvtani szabályokra, hanem fordítva: a nyelvtani szabályokat kell leképezni Xcore elemekre. Többféle Xtext nyelvtani szabály is visszaadhat ugyanolyan Xcore objektumot, nincs egy-az-egyes megfeleltetés a két reprezentáció között. A nyelvtant a WebTest nyelvi specifikációban szereplő kódrészletek alapján kell kialakítani. A nyelvtannak alkalmasnak kell lennie a **webtest.example** projektben szereplő **.wt** kiterjesztésű fájlok feldolgozására is.

Segítségképpen megadunk néhány nyelvtani szabályt:

```
grammar webtest.dsl.WebTestDsl with org.eclipse.xtext.common.Terminals

import "webtest.model"

Main: 'webtest' testClass+=ID ('.' testClass+=ID)* declarations+=Declaration* body=BlockStatement;

// ...

Expression: NotExpression;
NotExpression returns Expression: {NotExpression} 'not' operand=ConditionalExpression | ConditionalExpression;
ConditionalExpression returns Expression: IsExpression | ContainsExpression | ExistsExpression | SimpleExpression;
IsExpression: left=ReferenceExpression 'is' right=SimpleExpression;
ContainsExpression: left=ReferenceExpression 'contains' right=SimpleExpression;
ExistsExpression: operand=ReferenceExpression 'exists';
SimpleExpression returns Expression: ReferenceExpression | StringExpression | IntegerExpression | BooleanExpression;
ReferenceExpression returns SimpleExpression: ElementExpression | VariableExpression;
ElementExpression: tag=ID label=STRING;
VariableExpression: variable=[Variable];
StringExpression: value=STRING;
IntegerExpression: value=INT;
BooleanExpression: value?='true' | {BooleanExpression} 'false';
```

A többi szabály elkészítése a laborfeladat része. A WebTest nyelv közös részét mindenkinek meg kell valósítania. A bővítmények közül csak a kiosztott 2 darab bővítményt kell kötelezően támogatni, a többi bővítmény támogatása opcionális.

A szabályok kialakításánál az alábbiakra célszerű ügyelni:

* Egy nyelvtani szabály neve meg kell egyezzen az Xcore metamodellben szereplő neki megfelelő osztály nevével. Ha a név nem egyezik, akkor a `returns` kulcsszóval lehet specifikálni, hogy az adott nyelvtani szabály melyik Xcore osztályból készítsen példányt.
* Egy szabály csak egy Xcore osztályt tud példányosítani, és egy objektum példányosítása nem darabolható fel több nyelvtani szabályra.
* Ha egy szabályból nem feltétlenül készülne Xcore objektum (pl. mert egyetlen property sem kap értéket), de mégis mindenképpen szeretnénk ilyen objektumot létrehozni, akkor kapcsos zárójelek között (`{...}`) kell megadni a megfelelő Xcore osztály nevét.
* Egy Xcore osztály tulajdonságainak (property-jeinek) az `=`, `+=` és `?=` operátorokkal adhatunk értéket. A nyelvtanban szereplő property nevek meg kell egyezzenek az Xcore osztályban szereplő property-k neveivel.
* Opcionális elemek jelenléte a `?=` értékadással értékelhető ki.
* Lista property-khez a `+=` értékadással lehet elemeket hozzáadni.
* Hivatkozhatunk más objektumokra a `[...]` szintaxissal. A szögletes zárójelek között meg kell adni a hivatkozott objektum típusát. A hivatkozásokat az Xtext a hivatkozott objektum neve alapján oldja fel. Ha az Xtext alapértelmezett feloldási stratégiája nem megfelelő, akkor azt később felüldefiniálhatjuk (ld. később: scoping)
* Ahhoz, hogy valamire lehessen kereszthivatkozással mutatni, az adott objektumnak kell hogy legyen `name` property-je, amin keresztül a hivatkozás megtörténik. Az Xcore metamodell már eleve így lett kialakítva, hogy ennek a feltételnek megfeleljen.

Miután elkészültetek a nyelvtani szabályokkal, illetve ha bármikor a későbbiekben változtattok rajtuk, le kell futtatnotok a **webtest.dsl** projekten belül az **src** könyvtár alatt a **webtest.dsl** csomagban található **GenerateWebTestDsl.mwe2** generátort. Ehhez kattintsatok jobb gombbal a generátoron és válasszátok a **Run as > MWE2 Workflow** menüpontot!

Elképzelhető, hogy a workflow lefutása során hibákat jelez (pl. balrekurzió vagy több alternatíva is illeszkedik ugyanolyan mintára). Ha hiba van a nyelvtanban, akkor hibásak lesznek a generált fájlok. Éppen ezért, a nyelvtanban lévő hibákat mindenképpen ki kell javítani! Hibás nyelvtannal a labor összes további részfeladatának megoldása elromolhat!

A generálás után kattintsatok jobb gombbal a **webtest.dsl** projekten, és válasszátok a **Run as > Eclipse application** menüpontot! Ekkor egy másik, ún. **Runtime Eclipse** nyílik meg, amelyben a **.wt** kiterjesztésű fájlokban szerkeszthetők a WebTest nyelvnek megfelelő programok. Ezekben kipróbálhatjátok az Xtext fordítótok aktuális működését.

## Ellenőrzés

A nyelvtan helyes működését a **webtest.dsl.tests** projekt JUnit tesztként való futtatásával (**Run as > JUnit Test**) is ellenőrizhetitek a **ParseTests**, **ParseExtensionsTests** és **ParseExamplesTests** tesztelő osztályok segítségével.

## Feltöltendő

Ha ezt a részfeladatot sikerült megoldani, készítsetek screenshot-okat és töltsétek fel a képeket a saját repótokon belül a **homeworks/hw1** mappába az alábbiakról:

* A **Runtime Eclipse**-ben megnyitott legalább 20 soros **.wt** kiterjesztésű fájl, amely legalább 5 közös nyelvi elemre és a 2 bővítményre is tartalmaz példát.
* Az **Eclipse**-ben menyitott **WebTest.xtext** fájlban a 2 bővítmény Xtext nyelvtani reprezentációja.
* A **ParseTests**, **ParseExtensionsTests** és **ParseExamplesTests** összes tesztjének sikeres lefutása
