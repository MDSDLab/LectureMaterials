# Mintapélda kipróbálása

A **webtest.example** alkalmazás néhány mintapéldát tartalmaz, amelyek megmutatják, hogyan is kell viselkednie a végső alkalmazásnak.

A projekten belül a **webtest** könyvtár tartalmazza a WebTest nyelven megírt programkódokat, az **src-gen/test/java** könyvtár pedig azokat a Java kódokat, amelyek az egyes WebTest fájlok által leírt viselkedést mutatják be. *Fontos kihangsúlyozni, hogy ezek a Java kódok manuálisan, kézzel lettek megírva, hogy mintaként szolgáljanak a működés bemutatására. A laborfeladat végső megoldásában nem pontosan ezeket a Java kódokat kell generálni, hanem olyan Java kódokat, amelyek viselkedése megegyezik a manuálisan megírt Java kódokéval!*

Ahhoz, hogy az alkalmazást futtatni lehessen, szükséges, hogy legyen a számítógépeteken Chrome böngésző, és a böngésző verziójával megegyező [chromedriver](https://chromedriver.chromium.org/downloads)-t is le kell töltenetek, és be kell másolnotok a `c:\Programs\chromedriver\` mappába. Ha más mappában helyeznétek el a `chromedriver.exe` programot, akkor az **src/test/java/webtest/selenium/api/SeleniumTest.java** fájlban írjátok át a **DEFAULT_CHROME_DRIVER_LOCATION** konstans értékét!

Kattintsatok jobb gombbal a **webtest.example** projekten, és válasszátok a **Run As > JUnit Test** menüpontot! Figyeljétek meg, hogy a Selenium hogyan vezérli a böngészőt a JUnit teszteken keresztül! Vizsgáljátok meg a logokat és az **output** könyvtár tartalmát is!

A laborfeladat végére a következő eredményig kell majd eljutni:

* Tudjunk Eclipse-ben egy varázsló segítségével ilyen struktúrájú projektet létrehozni.
* Ha egy ilyen projektben a **webtest** könyvtáron belül létrehozunk vagy szerkesztünk egy **.wt** kiterjesztésű fájlt a WebTest programnyelvnek megfelelő tartalommal, akkor az **src-gen/test/java** könyvtárban automatikusan álljon elő a WebTest kódnak megfelelő JUnit teszt Java kódként.
* A projektet el lehessen indítani a **Run As > JUnit Test** menüponttal.
* A cél az, hogy használatkor csak a **.wt** kiterjesztésű fájlokat kelljen szerkeszteni, rajtuk kívül semmilyen más fájlhoz ne kelljen hozzányúlni.
