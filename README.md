# Testing

src
 └── test
      ├── java
      │     ├── base
      │     │     └── BaseTest.java
      │     ├── pages
      │     │     └── HomePage.java
      │     └── tests
      │           └── RentTabTest.java
      └── resources
            └── testng.xml

Homepage
package pages;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

public class HomePage {

    WebDriver driver;
    WebDriverWait wait;

    public HomePage(WebDriver driver, WebDriverWait wait) {
        this.driver = driver;
        this.wait = wait;
        PageFactory.initElements(driver, this);
    }

    @FindBy(xpath = "//div[text()='Rent']")
    WebElement rentTab;

    @FindBy(xpath = "//div[contains(@class,'nb__1PU')]//div[text()='Rent']")
    WebElement rentTabActive;

    public boolean isRentTabVisible() {
        wait.until(ExpectedConditions.visibilityOf(rentTab));
        return rentTab.isDisplayed();
    }

    public void clickRentTab() {
        wait.until(ExpectedConditions.elementToBeClickable(rentTab)).click();
    }

    public boolean isRentTabActive() {
        wait.until(ExpectedConditions.visibilityOf(rentTabActive));
        return rentTabActive.isDisplayed();
    }
}
renttesttab
package tests;

import io.github.bonigarcia.wdm.WebDriverManager;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.testng.Assert;
import org.testng.annotations.Test;
import pages.HomePage;

import java.time.Duration;

public class RentTabTest {

    @Test
    public void verifyRentTab() {

        // 1. Setup driver directly in test
        WebDriverManager.chromedriver().setup();
        WebDriver driver = new ChromeDriver();
        driver.manage().window().maximize();

        // 2. Open website
        driver.get("https://www.nobroker.in");

        // 3. Wait object
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));

        // 4. Page Object
        HomePage homePage = new HomePage(driver, wait);

        // 5. Assertions
        Assert.assertTrue(homePage.isRentTabVisible(), "Rent tab is NOT visible!");
        homePage.clickRentTab();
        Assert.assertTrue(homePage.isRentTabActive(), "Rent tab is NOT active after click!");

        // 6. Close browser
        driver.quit();
    }
}
tesngxml

<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd" >
<suite name="NoBroker Test Suite">

    <test name="Rent Tab Test">
        <classes>
            <class name="tests.RentTabTest" />
        </classes>
    </test>

</suite>


pom <dependencies>

    <!-- Selenium -->
    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>4.22.0</version>
    </dependency>

    <!-- TestNG -->
    <dependency>
        <groupId>org.testng</groupId>
        <artifactId>testng</artifactId>
        <version>7.9.0</version>
        <scope>test</scope>
    </dependency>

    <!-- WebDriverManager -->
    <dependency>
        <groupId>io.github.bonigarcia</groupId>
        <artifactId>webdrivermanager</artifactId>
        <version>5.8.0</version>
    </dependency>

</dependencies>








Pom xml 

<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.automation</groupId>
    <artifactId>nobroker-tests</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>

        <!-- Selenium -->
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>4.22.0</version>
        </dependency>

        <!-- TestNG -->
        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>7.9.0</version>
            <scope>test</scope>
        </dependency>

        <!-- WebDriverManager -->
        <dependency>
            <groupId>io.github.bonigarcia</groupId>
            <artifactId>webdrivermanager</artifactId>
            <version>5.8.0</version>
        </dependency>

    </dependencies>

</project>

------------------------------------------------


<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>nobroker</groupId>
    <artifactId>selenium-test</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>

        <!-- Selenium -->
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>4.23.0</version>
        </dependency>

        <!-- TestNG -->
        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>7.10.2</version>
            <scope>test</scope>
        </dependency>

    </dependencies>
</project>


<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="NoBroker UI Tests">
    <test name="Home Page Tabs">
        <classes>
            <class name="tests.HomePageTest"/>
        </classes>
    </test>
</suite>


package pages;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;

public class HomePage {

    WebDriver driver;

    public HomePage(WebDriver driver) {
        this.driver = driver;
        PageFactory.initElements(driver, this);
    }

    @FindBy(xpath = "//div[text()='Rent']")
    WebElement rentTab;

    @FindBy(xpath = "//div[text()='Buy']")
    WebElement buyTab;

    @FindBy(xpath = "//div[text()='Commercial']")
    WebElement commercialTab;

    public void clickRentTab() {
        rentTab.click();
    }

    public void clickBuyTab() {
        buyTab.click();
    }

    public void clickCommercialTab() {
        commercialTab.click();
    }

    public boolean isRentTabSelected() {
        return rentTab.getAttribute("class").contains("active");
    }

    public boolean isBuyTabSelected() {
        return buyTab.getAttribute("class").contains("active");
    }

    public boolean isCommercialTabSelected() {
        return commercialTab.getAttribute("class").contains("active");
    }
}


package tests;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.Assert;
import org.testng.annotations.Test;
import pages.HomePage;

import java.time.Duration;

public class HomePageTest {

    @Test
    public void verifyTabsSelection() {

        WebDriver driver = new ChromeDriver();
        driver.manage().window().maximize();
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
        driver.get("https://www.nobroker.in/");

        HomePage home = new HomePage(driver);

        // ✅ Test Rent tab
        home.clickRentTab();
        Assert.assertTrue(home.isRentTabSelected(), "Rent tab is NOT selected!");

        // ✅ Test Buy tab
        home.clickBuyTab();
        Assert.assertTrue(home.isBuyTabSelected(), "Buy tab is NOT selected!");

        // ✅ Test Commercial tab
        home.clickCommercialTab();
        Assert.assertTrue(home.isCommercialTabSelected(), "Commercial tab is NOT selected!");

        driver.quit();
    }
}


