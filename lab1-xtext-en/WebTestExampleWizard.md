# Example: wizard

The example below operates a web page that uses a wizard to enter data:

```
webtest example.WizardTest

page Name
  element firstName = input "First name..."
  element lastName = input "Last name..."
  element next = button "Next"
end

page ContactInfo
  element email = input "E-mail..."
  element phone = input "Phone..."
  element next = button "Next"
  element previous = button "Previous"
end

page Birthday
  element day = input "dd"
  element month = input "mm"
  element year = input "yyyy"
  element next = button "Next"
  element previous = button "Previous"
end

page LoginInfo
  element username = input "Username..."
  element password = input "Password..."
  element submit = button "Submit"
  element previous = button "Previous"
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

Each page of the wizard gets its own object model: *Name*, *ContactInfo*, *Birthday* and *LoginInfo*. We fill the appropriate fields on the appropriate pages and we navigate between pages using the *Next* and *Previous* buttons. Finally, we press *Submit* to upload the supplied data.

