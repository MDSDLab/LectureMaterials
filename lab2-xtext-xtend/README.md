# 2. labor - Xtext fordító és Xtend kódgenerátor

## A laborfeladat elvégzésének lépései

1. Csináljátok végig az [Xtext](https://eclipse.dev/Xtext/documentation/102_domainmodelwalkthrough.html) és [Xtend](https://eclipse.dev/Xtext/documentation/103_domainmodelnextsteps.html) tutorialokat!
2. Az [mdsd-2023-lab2-xtext-xtend](https://github.com/MDSDLab/mdsd-2023-lab2-xtext-xtend) repóból másoljátok át a **webtest** névvel kezdődő projekteket közvetlenül a saját repótok gyökerébe a már meglévő **webtest.model** és **webtest.model.tests** projektek mellé!
3. Oldjátok meg az alábbiakban leírt feladatokat, figyelembe véve a számotokra kiosztott bővítményeket!
4. Készítsetek egy **hw2** nevű **tag**-et az utolsó commitra!

## Fordító és kódgenerátor készítése a WebTest nyelvhez

A laborfeladat célja Xtext segítségével fordító és Eclipse IDE támogatás, valamint Xtend segítségével kódgenerátor készítése a WebTest nyelvhez. A fordító feladata, hogy a szöveges WebTest leírásból az előző labor során kialakított Xcore metamodellnek megfelelő modellt építsen fel, és ezen a modellen különböző ellenőrzéseket végezzen. Az IDE támogatás célja, hogy leegyszerűsítse a WebTest fájlok szerkesztését. A kódgenerátor pedig a WebTest fájlokból JUnit teszteket állít elő úgy, hogy azokat közvetlenül futtatni lehessen.

A WebTest nyelv közös részét mindenkinek meg kell valósítania. A bővítmények közül csak a kiosztott 2 darab bővítményt kell kötelezően támogatni, a többi bővítmény támogatása opcionális.

## Feladat leírása

A laborfeladat elvégzéséhez nyissátok meg az alábbi projekteket Eclipse alatt:

* **webtest.dsl**
* **webtest.dsl.ide**
* **webtest.dsl.tests**
* **webtest.dsl.ui**
* **webtest.dsl.ui.tests**
* **webtest.example**
* **webtest.generator**
* **webtest.model**
* **webtest.model.tests**

A **webtest.dsl** projektben az **src** könyvtár alatt a **webtest.dsl** csomagban szereplő **WebTestExtensions** osztályban állítsátok **false**-ra az azon bővítményekhez tartozó értékeket, amelyeket nem kell megvalósítanotok. Így csak olyan tesztek lesznek engedélyezve, amelyek számotokra relevánsak.

A laborfeladat elkészítése több lépésből áll. Az egyes lépések az alábbi módon csoportosíthatók.

Mintapélda:

0. [Mintapélda kipróbálása](Example.md)

Xtext fordító:

1. [Az Xtext nyelvtan elkészítése](Xtext.md)
2. [TIPP (opcionális): Extra információk hozzákötése az Xcore metamodellhez](XcoreExtra.md)
3. [Névelemzés (name analysis, scoping, validation)](NameAnalysis.md)
4. [Típuselemzés (type analysis, validation)](TypeAnalysis.md)

Xtext IDE támogatás:

5. [Kódvázlat (outline view, label provider)](Outline.md)
6. [Kódszínezés (syntax coloring, highlighting)](Highlighting.md)
7. [Projekt varázsló (project wizard)](ProjectWizard.md)

Xtend generátor:

8. [Kódgenerálás (code generation)](CodeGeneration.md)

## Megoldás ellenőrzése

A **webtest.dsl.tests** projekt azt teszteli, hogy az elkészített Xtext fordító megfelel-e a laborfeladat követelményeinek.

A **webtest.dsl.ui.tests** projekt azt teszteli, hogy az elkészített Xtext IDE támogatás megfelel-e a laborfeladat követelményeinek.

A laborfeladat megoldása akkor tekinthető sikeresnek, ha az összes releváns teszt sikeresen le tud futni, és a generátor is megfelelően működik. Azonban a laborfeladat megoldásának alaposabb kiértékeléséhez a itt szereplő (előre kiadott) teszteken túl további tesztek is futtatásra kerülnek leadás után!

Ha az összes részfeladattal végeztetek, az alábbi működés az elvárt:

1. Indítsunk el a **Runtime Eclipse**-et
2. Készítsünk egy új **WebTest projektet** a varázsló segítségével
3. A projekt **webtest** könyvtárába készítsünk egy új **.wt** kiterjesztésű fált
4. Írjunk WebTest nyelven teszteket a **.wt** kiterjesztésű fájlba, és közben élvezzük a saját Xtext alapú IDE támogatásunk előnyeit: kódszínezés, outline nézet, validációk, stb.
5. Mentsük el a **.wt** kiterjesztésű fájlt. Ekkor automatikusan előáll a neki megfelelő JUnit tesztet tartalmazó Java kód.
6. Kattintsunk jobb gombbal a projekten, és válasszuk a **Run As > JUnit Test** menüpontot. Ennek hatására a JUnit teszt egy Selenium által vezérelt böngészőn keresztül végrehajtja a WebTest nyelven leírt utasításainkat.

## Referenciák

Hasznos linkek az Xtext és Xtend használatához:

* Az előző féléves Xtext-Xtend gyakorlat [útmutatója](../lab1-xcore/images/GY4-XtextXtend-Utmutato.pdf)
* [Xtext](https://eclipse.dev/Xtext/documentation/index.html)
* [Xtend](https://eclipse.dev/Xtext/xtend/documentation/index.html)
