# 4. labor (2. rész) - HTML weboldal szerkesztő Blockly alapon

## A laborfeladat elvégzésének lépései

1. Olvassátok el és lehetőség szerint csináljátok végig a Blockly [Tutorial](materials/blockly_tutorial.pdf) feladatot! A leírásban szereplő feladatok nem kapcsolódnak a laborfeladathoz, de segítenek megtenni az első lépéseket a Blockly világában. A feladatok elvégezhetőek a kiadott Blockly verzióban.
2. Az [mdsd-2023-lab4-blockly](https://github.com/MDSDLab/mdsd-2023-lab4-blockly) repóból másoljátok át a **Blockly** mappát közvetlenül a saját repótok gyökerébe!
3. Oldjátok meg az alábbiakban leírt feladatokat, figyelembe véve a számotokra kiosztott [bővítményeket](extensions.md)!
4. Készítsetek egy **hw4-blockly** nevű **tag**-et az utolsó commitra!

## HTML szerkesztő nyelv, Blockly-alapú editor és kódgenerátor készítése

A laborfeladat célja a Blockly környezetben összeállítani egy nyelvet (Blockly4HTML), amivel olyan weboldalak állíthatóak össze, amelyek az alapvető HTML elemeket tartalmazzák. A Blocklyban leírt weboldalhoz tartozó logikát le is kell generálni a kiadott kódgenerátor testreszabásával. 

*A feladat csak lazán kapcsolódik a WebTest nyelvhez, az ANTLR feladattal ellentétben itt nem cél, hogy a WebTest által közvetlenül használható weboldal álljon elő a generálás végén.*

A Blockly4HTML nyelv [közös részét](controls.md) mindenkinek meg kell valósítania. A [bővítmények](extensions.md) közül csak az egyetlen [kiosztott](ExtensionsTable.md) bővítményt kell kötelezően támogatni.

## Feladat leírása

A laborfeladat elvégzéséhez nyissátok meg a kiadott kezdőprojektet (az *index.html* oldalt megnyitva) és olvassátok el a hozzá készített [leírást](init_example.md)! Ez egy minimális példát tartalmaz, ahol az alapvető funkciók kipróbálhatóak, de a megvalósítandó HTML elemek még hiányoznak. A Blockly nyelv egyetlen elemet tartalmaz, de kipróbálható a kódgenerálás, ill. a könnyebb használat érdekében a végső állapot (működő weboldal-részlet) is legenerálható, valamint letölthető. 

A laborfeladat megoldása akkor tekinthető késznek, ha az összes felsorolt HTML elem ([alap elemek](controls.md) és a kijelölt [bővítmény](extensions.md)) lemodellezhető (tartalmazza a nyelv és az editorban fel lehet használni) és helyes (működő, szemantikailag megfelelő) HTML kód generálódik belőlük. A Blockly természete miatt automatikus teszteseteket ennél a feladatnál nem adunk közre, de manuálisan ellenőrizhető, hogy a megoldás kielégíti-e a követelményeket. 

### Pontozás

A 4. labor két különálló feladatból áll; az **[ANTLR](https://github.com/MDSDLab/LectureMaterials/blob/main/lab4-antlr/README.md) feladatra 15 pontot**, a **Blockly feladatra 10 pontot** lehet maximum szerezni. A Blockly feladat pontozása a következőképpen oszlik el:

| Közös részek | Kódgenerálás | Bővítmény |
| :-: | :-: | :-: |
| 3 pont | 4 pont | 3 pont |

## Referenciák

Hasznos linkek a feladat megoldásához:

* Blockly tutorial [útmutató](materials/elosztott_labor.pdf)
* Néhány [tipp](hints.md) a feladatok megoldásához
* Blockly [Block factory](https://blockly-demo.appspot.com/static/demos/blockfactory/index.html)
* Blockly [documentation](https://developers.google.com/blockly/guides/overview)
* HTML leíró nyelv Blocklyban (részben hasonló a laborfeladathoz) [link](http://blocklyhtml.zgtm.de/)
* Template literal (kódgeneráláshoz) [link](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)
* Blockly field demo [link](https://google.github.io/blockly-samples/)
* Input validation [link](http://bekawestberg.me/blog/types-1/)
* Custom validation [link](https://blocklycodelabs.dev/codelabs/validation-and-warnings/index.html#0)
