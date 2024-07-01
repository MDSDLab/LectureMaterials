# Mintapélda kipróbálása

A **webtest.example** alkalmazás néhány mintapéldát tartalmaz, amelyek megmutatják, mi is a végső cél, hogyan is kell viselkednie a végső alkalmazásnak az első és a második labor után.

A projekten belül a **webtest** könyvtár tartalmazza a WebTest nyelven megírt programkódokat, az **src-gen/test/java** könyvtár pedig azokat a Java kódokat, amelyek az egyes WebTest fájlok által leírt viselkedést mutatják be. 
*Fontos kihangsúlyozni, hogy ezek a Java kódok manuálisan, kézzel lettek megírva, hogy mintaként szolgáljanak a működés bemutatására. A következő laboron kell majd elkészíteni azt a generátort, amely a WebTest kódokból hasonló viselkedésű Java kódokat állít elő.*

Ahhoz, hogy az alkalmazást futtatni lehessen, szükséges, hogy legyen a számítógépeteken Chrome böngésző, és a böngésző verziójával megegyező [chromedriver](https://chromedriver.chromium.org/downloads)-t is le kell töltenetek, és be kell másolnotok a `c:\Programs\chromedriver\` mappába. Ha más mappában helyeznétek el a `chromedriver.exe` programot, akkor az **src/test/java/webtest/selenium/api/SeleniumTest.java** fájlban írjátok át a **DEFAULT_CHROME_DRIVER_LOCATION** konstans értékét!

Kattintsatok jobb gombbal a **webtest.example** projekten, és válasszátok a **Run As > JUnit Test** menüpontot! Figyeljétek meg, hogy a Selenium hogyan vezérli a böngészőt a JUnit teszteken keresztül! Vizsgáljátok meg a logokat és az **output** könyvtár tartalmát is!
