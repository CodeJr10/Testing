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


Base Test Java 
package base;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;

import java.time.Duration;

public class BaseTest {

    protected WebDriver driver;
    protected WebDriverWait wait;

    @BeforeMethod
    public void setUp() {
        driver = new ChromeDriver();
        driver.manage().window().maximize();

        wait = new WebDriverWait(driver, Duration.ofSeconds(10));
        driver.get("https://www.nobroker.in");
    }

    @AfterMethod
    public void tearDown() {
        if (driver != null) {
            driver.quit();
        }
    }
}
Homepage java
package pages;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.openqa.selenium.support.ui.ExpectedConditions;

public class HomePage {

    WebDriver driver;
    WebDriverWait wait;

    // Rent tab
    @FindBy(xpath = "//div[text()='Rent']")
    WebElement rentTab;

    // Active selected Rent tab
    @FindBy(xpath = "//div[contains(@class,'nb__1PU')]//div[text()='Rent']")
    WebElement activeRentTab;

    public HomePage(WebDriver driver, WebDriverWait wait) {
        this.driver = driver;
        this.wait = wait;
        PageFactory.initElements(driver, this);
    }

    public boolean isRentTabVisible() {
        wait.until(ExpectedConditions.visibilityOf(rentTab));
        return rentTab.isDisplayed();
    }

    public void clickRentTab() {
        wait.until(ExpectedConditions.elementToBeClickable(rentTab)).click();
    }

    public boolean isRentTabActive() {
        wait.until(ExpectedConditions.visibilityOf(activeRentTab));
        return activeRentTab.isDisplayed();
    }
}
Rent test tab 
package tests;

import base.BaseTest;
import pages.HomePage;
import org.testng.Assert;
import org.testng.annotations.Test;

public class RentTabTest extends BaseTest {

    @Test
    public void verifyRentTabFunctionality() {

        HomePage homePage = new HomePage(driver, wait);

        // 1. Validate Rent tab is visible
        Assert.assertTrue(homePage.isRentTabVisible(),
                "Rent tab should be visible on homepage");

        // 2. Click Rent tab
        homePage.clickRentTab();

        // 3. Validate Rent tab is activated
        Assert.assertTrue(homePage.isRentTabActive(),
                "Rent tab should be active after clicking");
    }
}
testng xml 
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="NoBroker Test Suite">
    <test name="Rent Tab Test">
        <classes>
            <class name="tests.RentTabTest"/>
        </classes>
    </test>
</suite>


Maven dependencies

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

    <!-- WebDriver Manager (Optional but recommended) -->
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

