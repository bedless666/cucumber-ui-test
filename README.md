# Cucumber UI Test Framework

## Deskripsi
Framework pengujian UI berbasis Cucumber, Java, Gradle, dan Selenium.

## Instalasi
1. Clone repositori:
   ```sh
   git clone https://github.com/bedless666/cucumber-ui-test.git
   ```

2. Masuk ke direktori proyek:
   ```sh
   cd cucumber-ui-test
   ```

3. Jalankan pengujian:
   ```sh
   ./gradlew test
   ```

## Struktur Proyek
```
cucumber-ui-test/
│-- src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/pages/  # Implementasi Page Object Model (POM)
│   ├── test/
│   │   ├── java/
│   │   │   ├── com/example/stepdefinitions/  # Step Definitions untuk Cucumber
│   │   │   └── com/example/runners/  # Test Runner untuk Cucumber
│   │   ├── resources/
│   │   │   ├── features/  # Test case Gherkin
│   │   │   └── config/  # Konfigurasi tambahan jika diperlukan
│-- build.gradle  # Konfigurasi Gradle
│-- README.md  # Dokumentasi proyek
```

## Implementasi
### 1. Implementasi Page Object Model (POM)
Page Object Model (POM) digunakan untuk merepresentasikan setiap halaman UI sebagai kelas Java.

Contoh: `LoginPage.java`
```java
package com.example.pages;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;

public class LoginPage {
    private WebDriver driver;
    
    private By usernameField = By.id("username");
    private By passwordField = By.id("password");
    private By loginButton = By.id("login");
    
    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }
    
    public void enterUsername(String username) {
        driver.findElement(usernameField).sendKeys(username);
    }
    
    public void enterPassword(String password) {
        driver.findElement(passwordField).sendKeys(password);
    }
    
    public void clickLogin() {
        driver.findElement(loginButton).click();
    }
}
```

### 2. Test Case Gherkin
Contoh file `login.feature` di `src/test/resources/features/`:
```gherkin
Feature: Login Functionality

  Scenario: Successful login with valid credentials
    Given User is on login page
    When User enters valid username and password
    And User clicks the login button
    Then User should be redirected to the dashboard

  Scenario: Unsuccessful login with invalid credentials
    Given User is on login page
    When User enters invalid username or password
    And User clicks the login button
    Then User should see an error message
```

### 3. Implementasi Step Definitions
Contoh `LoginSteps.java` di `src/test/java/com/example/stepdefinitions/`:
```java
package com.example.stepdefinitions;

import com.example.pages.LoginPage;
import io.cucumber.java.en.Given;
import io.cucumber.java.en.Then;
import io.cucumber.java.en.When;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import static org.junit.jupiter.api.Assertions.assertTrue;

public class LoginSteps {
    WebDriver driver = new ChromeDriver();
    LoginPage loginPage = new LoginPage(driver);

    @Given("User is on login page")
    public void userIsOnLoginPage() {
        driver.get("https://example.com/login");
    }

    @When("User enters valid username and password")
    public void userEntersValidCredentials() {
        loginPage.enterUsername("testuser");
        loginPage.enterPassword("password123");
    }

    @When("User clicks the login button")
    public void userClicksLoginButton() {
        loginPage.clickLogin();
    }

    @Then("User should be redirected to the dashboard")
    public void userIsRedirectedToDashboard() {
        assertTrue(driver.getCurrentUrl().contains("dashboard"));
        driver.quit();
    }
}
```

## Cara Menjalankan Cucumber
Jalankan perintah berikut untuk menjalankan pengujian Cucumber:
```sh
./gradlew test
```

## Dependencies di `build.gradle`
```gradle
plugins {
    id 'java'
}

group 'com.example'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.seleniumhq.selenium:selenium-java:4.5.0'
    implementation 'org.seleniumhq.selenium:selenium-chrome-driver:4.5.0'
    testImplementation 'io.cucumber:cucumber-java:7.14.0'
    testImplementation 'io.cucumber:cucumber-junit:7.14.0'
    testImplementation 'org.junit.jupiter:junit-jupiter:5.9.0'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}

test {
    useJUnitPlatform()
}
```

## Konfirmasi Checklist
✅ Repositori GitHub sudah dibuat dan public
✅ Kode sumber sudah mencakup framework dan implementasi
✅ Page Object Model 
✅ Test case Gherkin
✅ Step Definitions 
✅ Cucumber dapat dijalankan dengan `./gradlew test`
✅ README.md
