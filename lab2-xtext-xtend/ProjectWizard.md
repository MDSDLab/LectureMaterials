# Projekt varázsló (project wizard)

Ahhoz, hogy a WebTest fájlokból előálló JUnit tesztek végrehajthatók legyenek, a WebTest fájlokat egy megfelelő szerkezetű projektbe kell beágyazni (ld. **webtest.example** projekt).

Segítségképpen a **webtest.dsl.ui** projektben már elkészítettünk egy olyan varázslót (**src/webtest/dsl/ui/wizard/WebTestDslProjectTemplateProvider.xtend**), ami Selenium alapú JUnit tesztek futtatására alkalmas projektet állít elő. Ebben a fájlban két osztály található: **WebTestDslProjectTemplateProvider** és **WebTestProject**. A kívánt projekt tényleges létrehozását a **WebTestProject** osztály végzi, amely a következő könyvtárakat és fájlokat hozza létre:

* **src/main/java**: egy Maven projekt esetén ez tartalmazza a Java forrásfájlokat
  * **.gitignore**: a Git verziókezelő konfigurációs fájlja (üres, mert csak arra szolgál, hogy az **src/main/java** könyvtár megmaradjon verziókezelés alatt)
* **src/main/resources**: egy Maven projekt esetén ez tartalmazza a konfigurációs fájlokat
  * **logback.xml**: az slf4j naplózó rendszer konfigurációs fájlja
* **src/test/java**: egy Maven projekt esetén ez tartalmazza a teszt Java forrásfájlokat
  * **.gitignore**: a Git verziókezelő konfigurációs fájlja (üres, mert csak arra szolgál, hogy az **src/test/java** könyvtár megmaradjon verziókezelés alatt)
* **src/test/resources**: egy Maven projekt esetén ez tartalmazza a teszt konfigurációs fájlokat
  * **logback-test.xml**: az slf4j naplózó rendszer konfigurációs fájlja
* **webtest**: ide kell elhelyezni a **.wt** kiterjesztésű WebTest forrásfájlokat
  * **HelloWorld.wt**: egy Hello World jellegű példakód WebTest szintaxissal
* **.classpath**: az Eclipse egyik konfigurációs fájlja, amely a forráskódokat tartalmazó könyvtárakat határozza meg
* **.gitignore**: a Git verziókezelő konfigurációs fájlja, amely beállítja, hogy a **target** és **output** könyvtárak ne kerüljenek verziókezelés alá
* **pom.xml**: a Maven konfigurációs fájl, amely tartalmazza a JUnit 5 és a Selenium függőségeket

A fenti fájlok tartalmát a **webtest.generator** projekten belül az **webtest.generator.WebTestProjectGenerator** osztály állítja elő Xtend sablonok segítségével.

A **webtest** könyvtárban lévő **.wt** kierjesztésű fájlokból generált JUnit tesztek az **src-gen/test/java** könyvtába fognak kerülni, de ezt a lépést nem ez a projekt varázsló, hanem később, a WebTest fordítóhoz kapcsolt generátor fogja elvégezni.

A laborfeladat megoldása során elképzelhető, hogy ehhez a **WebTestProject** osztályhoz nem is kell hozzányúlnotok. Ha azonban vannak olyan segédosztályaitok (pl. a Selenium API felett egy csomagoló réteg), amelyekre a később generált JUnit tesztek építenek, akkor azokat célszerű itt, a **WebTestProject** osztály segítségével beilleszteni a varázsló által előállított projektbe. Ilyen osztály lehet például a **webtest.example** projektben szereplő **webtest.selenium.api.SeleniumTest** osztály, amelyet egy-az-egyben átvehettek, de akár saját ízléseteknek megfelelően átalakíthattok és továbbfejleszthettek. Ezen kívül készíthettek további segédosztályokat is (pl. az aktuális **context as ... end** kontextus követésére, vagy a **WebElement** osztály becsomagolására és felokosítására).

## Feltöltendő

Ha ezt a részfeladatot sikerült megoldani, készítsetek screenshot-okat és töltsétek fel a képeket a saját repótokon belül a **homeworks/hw2** mappába az alábbiakról:

* A **WebTestProject** osztály forráskódja a normál Eclipse-ben megnyitva
