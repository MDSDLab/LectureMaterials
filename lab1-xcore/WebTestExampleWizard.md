# Példa: varázsló

Az alábbi példa egy olyan weboldalt működtet, amely egy varázsló segítségével kéri be az adatokat:

```
webtest example.WizardTest

page Name
  set firstName to input "First name..."
  set lastName to input "Last name..."
  set next to button "Next"
end

page ContactInfo
  set email to input "E-mail..."
  set phone to input "Phone..."
  set next to button "Next"
  set previous to button "Previous"
end

page Birthday
  set day to input "dd"
  set month to input "mm"
  set year to input "yyyy"
  set next to button "Next"
  set previous to button "Previous"
end

page LoginInfo
  set username to input "Username..."
  set password to input "Password..."
  set submit to button "Submit"
  set previous to button "Previous"
end

open "https://www.w3schools.com/howto/howto_js_form_steps.asp"

context as Name
  fill firstName with "Agent"
  fill lastName with "Smith"
  click next
end

context as ContactInfo
  fill email with "smith@matrix.com"
  fill phone with "5551234"
  click next
end

context as Birthday
  fill day with "01"
  fill month with "01"
  fill year with "2000"
  click previous
end

context as ContactInfo
  fill phone with "5554321"
  click next
end

context as Birthday
  click next
end

context as LoginInfo
  fill username with "smith"
  fill password with "secret"
  click submit
end
```

A varázsló minden egyes oldala önálló modellt kap: *Name*, *ContactInfo*, *Birthday* és *LoginInfo*. A megfelelő oldalon kitöltjük a megfelelő mezőket, és a *Next* illetve a *Previous* feliratú gombokkal lépkedhetünk a varázsló oldalai között. Végül a *Submit* feliratú gombbal küldhetjük be a végleges adatokat.

