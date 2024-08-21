# Try the example

The **webtest.example** application contains some examples, which show the final goal: how the application should behave after the first and second labs.

Inside the project the **webtest** folder contains the program codes written in the WebTest language, while the **src/test/java** folder contains those Java codes which should result automatically from the WebTest files.

To be able to run the application, you need to have a Chrome browser on your computer along with a  [chromedriver](https://chromedriver.chromium.org/downloads) of the same version as the Chrome browser. Download the driver and copy it into the folder `c:\Programs\chromedriver\`. In case you put it into a different folder, make sure to updtate the **DEFAULT_CHROME_DRIVER_LOCATION** constant inside the **src/test/java/webtest/selenium/api/SeleniumTest.java** file!

Click on the **webtest.example** project with the right mouse button, and select **Run As > JUnit Test**! Observe, how Selenium controls the browser through the JUnit tests! Check the logs and the contents of the **output** folder, too!
