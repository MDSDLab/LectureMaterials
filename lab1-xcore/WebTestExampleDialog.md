# Példa: dialógus ablakok

Az alábbi példa dialógus ablakok használatát mutatja be:

```
webtest example.DialogTest

page ModalDialog
  set close to button "Close"
end

test largeModalTest
  open "https://getbootstrap.com/docs/4.0/components/modal/"
  click button "Large modal"
  context div "myLargeModalLabel" as ModalDialog
    click close
  end
end

test smallModalTest
  open "https://getbootstrap.com/docs/4.0/components/modal/"
  click button "Small modal"
  context div "mySmallModalLabel" as ModalDialog
    click close
  end
end
```

A fenti példában a *ModalDialog* reprezentál egy dialógus ablakot, amely a jobb felső sarkában egy bezáró (*close*) gombbal rendelkezik.

Attól függően, hogy a *Large modal* vagy a *Small modal* feliratú gombra kattintunk, más-más dialógus ablak jelenik meg, de mindkettő rendelkezik bezáró gombbal. A **context ... as** használatával kiválaszthatjuk azt a *div* elemet, amely a megfelelő dialógus ablakot reprezentálja, és a kiválasztott kontextusban alkalmazható a *ModalDialog* objektummodell.
