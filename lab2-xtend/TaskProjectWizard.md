# Projekt varázsló (project wizard)

Ebben a részfeladatban nincs semmi teendőtök, csak olvassátok el az alábbi leírást!

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
* **src/main/java/webtest/selenium/api/Page.java**: egy weboldal modell ősét reprezentáló segédosztály
* **src/main/java/webtest/selenium/api/PageElement.java**: egy weboldal elemet reprezentáló segédosztály
* **src/test/java/webtest/selenium/api/SeleniumTest.java**: a Selenium meghívását támogató segédosztály

A fenti fájlok tartalmát a **webtest.generator** projekten belül az **webtest.generator.WebTestProjectGenerator** osztály állítja elő Xtend sablonok segítségével.

A **webtest** könyvtárban lévő **.wt** kierjesztésű fájlokból generált JUnit tesztek az **src-gen/test/java** könyvtába fognak kerülni, de ezt a lépést nem ez a projekt varázsló, hanem a [következő részfeladatban](TaskCodeGeneration.md) elkészített generátor fogja elvégezni.

A varázslóhoz és az ahhoz kapcsolódó fájlokhoz ne nyúljatok hozzá, ne írjátok át őket! Ez a részfeladat csak előkészítette a következő részfeladat megoldását.

## Feltöltendő

Mivel ebben a részfeladatban nincs semmi teendő, itt nem kell képernyőképet feltölteni.
