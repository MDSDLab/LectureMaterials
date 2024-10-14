# 3. labor - VIATRA Modelllekérdezés és modelltranszformáció

## A laborfeladat elvégzésének lépései

0. Telepítsék a VIATRA-t az Eclipse-be (Help -> Eclipse Marketplace... -> Find: Viatra -> VIATRA 2.9.1 -> Install)
1. Csinálja végig a [VIATRA](https://github.com/ftsrg-edu/lecture-notes/wiki/2020_vql) és [Modelltranszformáció](https://github.com/ftsrg-edu/lecture-notes/wiki/2021_m2m) tutorialokat, vagy a [hivatalos VIATRA](https://eclipse.dev/viatra/documentation/tutorial.html) tutorialt!
2. Az [mdsd-2024-lab3-viatra](https://github.com/MDSDLab/mdsd-2024-lab3-viatra) repóból másolják át a **webtest.transformation** projektet a saját repójukba a már meglévő **webtest** névvel kezdődő projektek mellé!
3. Oldják meg az alábbiakban leírt feladatokat!
4. Készítsenek egy **hw3** nevű **tag**-et az utolsó commitra!

## Lekérdezések és transzformációk készítése a WebTest nyelvhez

A laborfeladat célja VIATRA segítségével lekérdezések és modelltranszformációk készítése a WebTest nyelvhez. A lekérdezések feladata, hogy a WebTest nyelv modelljein különböző ellenőrzéseket végezzenek. A modelltranszformációk feladata, hogy a WebTest nyelv modelljein különböző módosításokat végezzenek [mutációs tesztelés](https://en.wikipedia.org/wiki/Mutation_testing) céljából.

## Feladat leírása

A laborfeladat elvégzéséhez nyissák meg az alábbi projektet Eclipse alatt:

* **webtest.transformation**

Mintapélda:

0. [Mintapélda kipróbálása](Example.md)

Feladatok:

1. [Lekérdezések készítése](Viatra.md)
2. [Transzformációk készítése](Transformation.md)

## Megoldás ellenőrzése
A **test** mappán belül a **webtest.transformation.querytest** package-ben lévő **QueryTests.xtend** fájlban található generált tesztesetek a lekérdezések fejlesztését segítik. (Ezen tesztek futtatása kötelező, kérjük pontosan azokat a lekérdezés neveket használják, amiket megadtunk.) Azonban ezek a tesztek nem fednek le minden esetet, és a kiadott modellek sem feltétlenül tartalmaznak példát minden lekérdezés minden lehetséges illeszkedésére. A laborfeladat megoldásának alaposabb kiértékeléséhez a itt szereplő (előre kiadott) teszteken túl további tesztek is futtatásra kerülnek leadás után!
