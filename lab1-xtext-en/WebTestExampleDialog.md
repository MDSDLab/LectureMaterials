# Example: dialog boxes

The following example shows how dialog boxes can be represented in the WebTest language:

```
webtest example.DialogTest

page ModalDialog
  element close = button "Close"
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

In the example above the *ModalDialog* page model represents a modal dialog that has a *close* button.

Depending on whether we click on the *Large modal* or the *Small modal* button, a different dialog will appear, however, both of them has a close button. Using the **context ... as** construct we can select the *div* that represents the appropriate dialog, and in this context the *ModalDialog* object model can be used to click the close button.
