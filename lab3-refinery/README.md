# 3. labor – Refinery, modelllekérdezés és modellgenerálás

## A laborfeladat elvégzésének lépései

1. Ismerkedjetek meg a Refinery-ben elérhető parciális modellezési nyelvvel: [Refinery language reference](https://refinery.tools/learn/language/)
2. A labor fejlesztői környezete a Refinery webes szerkesztőjében érhető el: https://refinery.services/
3. A [mdsd-2025-lab3-refinery](https://github.com/MDSDLab/mdsd-2025-lab3-refinery) repository-ból másoljátok a **webtest-refinery** mappát a saját repository-tok gyökérkönyvtárába.
4. Oldjátok meg az alábbi feladatokat.
5. Készítsetek egy **hw3** git tag-et az utolsó commitra!

> [!TIP]
> Ne felejtsétek el időnként elmenteni a problem fájlokat a gépre (`Save` vagy `Save as...` gomb, `CTRL+S`).

## A labor célja

A labor célja, hogy megismerkedjetek a gráf-alapú modellezéssel és modellgenerálással. Ezen a laboron a [Refinery](https://refinery.tools/) segítségével predikátumokat fogtok létrehozni a WebTest nyelven alapuló modellekhez. A predikátumok célja, hogy különböző ellenőrzéseket hajtsanak végre a WebTest nyelv modellein.
Ezután a Refinery modellgeneráló funkcióját használva tesztmodelleket generáltok a WebTest nyelvhez a korábban definiált predikátumok felhasználásával.

## Fejlesztői környezet

A gráfpredikátumok fejlesztéséhez és a modellgenerálásához használhatjátok a [Refinery](https://refinery.tools/) gráfgenerátor keretrendszer online szerkesztőjét, amely elérhető a [▷Refinery](https://refinery.services/) oldalon, vagy [futtathattok saját szervert](https://refinery.tools/learn/docker/).


## Feladatok listája

Oldjátok meg a következő feladatokat:
1. [Gráfpredikátumok létrehozása](predicates.md)
2. [Tesztmodellek generálása](model-generation.md)
