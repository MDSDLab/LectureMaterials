# A transzformációs szabályok elkészítése

A **webtest.transformation** projekten belül az **queries** könyvtár alatt a **webtest.transformation.queries** csomagból nyissátok meg a **queries.vql** fájlt, és készítsétek el a lentebb definiált transzformációs szabályok előfeltételeit!

A **webtest.transformation** projekten belül az **src** könyvtár alatt a **webtest.transformation.rules** csomagból nyissátok meg a **MutationRules.xtend** fájlt, és készítsétek el a lentebb definiált transzformációs szabályokat! Fontos, hogy a szabályokhoz tartozó lekérdezések nevei megegyezzenek a zárójelben megadott nevekkel!

## WaitStatement törlése BlockStatement-ből (removeWaitStatement)
Keressünk olyan BlockStatement-et, melyben van WaitStatement, és töröljük ki belőle a WaitStatement-et!

## WaitStatement timeout érték megadása (setSeconds)
Keressünk olyan WaitStatement-et, melynek nincs _seconds_ timeout értéke, és adjunk neki 1 másodperces timeoutout!

## WaitStatement timeout érték módosítása (modifySeconds)
Keressünk olyan WaitStatement-et, melynek _seconds_ timeout értéke nagyobb mint 10, és felezzük meg a _seconds_ értékét! (Ha a _seconds_ új értéke nem egész szám, akkor kerekítsük lefelé!)
Vegyük figyelembe, hogy a _seconds_ értéke nem csak IntegerExpression lehet, hanem VariableExpression-be is be lehet csomagolva akár többször is! Ilyen esetben a struktúrát ne változtassuk, csak a becsomagolt IntegerExpression értékét írjuk át!

## PrintStatement törlése BlockStatement-ből (removePrintStatement)
Keressünk olyan BlockStatement-et, melyben van PrintStatement, és töröljük ki belőle a PrintStatement-et!

## URL módosítása (modifyURL)
Keressünk olyan StringExpression-t, melynek értékében bárhol szerepel a "https://" karaktersor, és cseréljük le "https://" karaktersort "http://"-re!

## URL módosítása 2 (modifyURL2)
Keressünk olyan StringExpression-t, mely a "https://www." karaktersorral kezdődik, és vágjuk le a "https://www." karaktersort a String elejéről.

