# Az Xtext nyelvtan elkészítése

A WebTest nyelv szöveges reprezentációjának feldolgozásához szükség van egy fordítóra. A fordító előállításához meg kell adni a szöveges reprezentáció formális leírását. Erre szolgál az Xtext nyelvtan. Az Xtext szintaxisról részletes leírás [itt található](https://eclipse.dev/Xtext/documentation/301_grammarlanguage.html).

A **webtest.dsl** projekten belül az **src** könyvtár alatt a **webtest.dsl** csomagból nyissátok meg a **WebTest.xtext** fájlt, és készítsétek el a WebTest nyelv nyelvtanát.

Segítségképpen megadunk néhány nyelvtani szabályt:

```
grammar webtest.dsl.WebTestDsl with org.eclipse.xtext.common.Terminals

import "webtest.model"

Main: 'webtest' testClass+=ID ('.' testClass+=ID)* declarations+=Declaration* body=BlockStatement;

// ...

Expression: BinaryExpression | UnaryExpression | SimpleExpression;
BinaryExpression returns Expression: IsExpression | ContainsExpression;
IsExpression: left=ReferenceExpression 'is' right=SimpleExpression;
ContainsExpression: left=ReferenceExpression 'contains' right=SimpleExpression;
UnaryExpression returns Expression: ExistsExpression | NotExpression;
ExistsExpression: operand=ReferenceExpression 'exists';
NotExpression: 'not' operand=Expression;
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

A generálás után kattintsatok jobb gombbal a **webtest.dsl** projekten, és válasszátok a **Run as > Eclipse application** menüpontot! Ekkor egy másik, ún. **Runtime Eclipse** nyílik meg, amelyben a **.wt** kiterjesztésű fájlokban szerkeszthetők a WebTest nyelvnek megfelelő programok. Ezekben kipróbálhatjátok az Xtext fordítótok aktuális működését.

## Feltöltendő

Ha ezt a részfeladatot sikerült megoldani, készítsetek screenshot-okat és töltsétek fel a képeket a saját repótokon belül a **homeworks/hw2** mappába az alábbiakról:

* A **Runtime Eclipse**-ben megnyitott legalább 20 soros **.wt** kiterjesztésű fájl, amely legalább 5 közös nyelvi elemre és a 2 bővítményre is tartalmaz példát.
* Az **Eclipse**-ben menyitott **WebTest.xtext** fájlban a 2 bővítmény Xtext nyelvtani reprezentációja.
