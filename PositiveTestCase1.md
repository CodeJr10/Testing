
RunTests java
```
package tests;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.testng.annotations.Test;

import io.github.bonigarcia.wdm.WebDriverManager;


public class runTest1 extends BaseSteps {
	
	WebDriver driver;
	RentTabTest rent;
	
	@Test
	public void verifyTests() throws InterruptedException {
		
		// block notifs
		ChromeOptions options = new ChromeOptions();
		options.addArguments("--disable-notifications");
		WebDriverManager.chromedriver().setup();		
		BaseSteps.setup();
		driver = new ChromeDriver(options);
		
		
		
		driver.manage().window().maximize();
		driver.get("https://www.nobroker.in/");
		
		rent = new RentTabTest(driver);
		
		
//		rent.selectRoom();
//		Thread.sleep(5000);
//		rent.selectRoomOption();
//		Thread.sleep(5000);
		rent.clickLocation();
		Thread.sleep(5000);
		rent.locationOptionClick();
		rent.clickRent();
		
		Thread.sleep(5000);
//		Thread.sleep(5000);

		rent.enterData("Vikhroli"); 
		Thread.sleep(5000);
		
		rent.clickOption();
		rent.clickSearch();
	}
}
```
RentTabTest

```
package tests;

import java.util.List;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;

public class RentTabTest {
	
	WebDriver driver;
	RentTabTest rent;

	// constructor

public RentTabTest(WebDriver driver) {
        this.driver = driver;
        PageFactory.initElements(driver, this);
    }


	// Rent Button
	@FindBy(xpath="//div[@class='cursor-pointer text-primary-color border-0 border-b-4 border-solid border-primary-color font-bold']") WebElement rentBtn;
	
	//LocationSelector - index 0
	@FindBy(css="div[class=\"css-v2nw5i-control nb-select__control\"]") WebElement location;
	
	// LocationOption
	@FindBy(xpath="//*[@id='react-select-2-option-0']") WebElement locationOption;
	
	//SearchInput
	@FindBy(xpath="//input[@id='listPageSearchLocality']") WebElement searchInput;
	
	//SearchOption
	@FindBy(xpath="//div[@class='address flex justify-start items-center w-92pe']") WebElement selectOption;
	
	//SearchButton
	@FindBy(xpath="//img[@alt='searchIcon']") WebElement searchButton;
	
	//SelectRoom
	@FindBy(css="#searchCity > div > div.css-1wy0on6.nb-select__indicators") List<WebElement> room;
	
	//SelectRoomOption
	@FindBy(xpath="//*[@id=\"react-select-6-option-1\"]") WebElement roomOption;

	String place = "Vikhroli";
	
	public void selectRoom() {
		if(room.size() > 1) {
			
			room.get(1).click();
		}else {
			System.out.println("Not enough room elements");
		}
	}
	
	public void selectRoomOption() {
		roomOption.click();
	}
	
	
	public void clickLocation() {
		location.click();
	}
	
	
	public void locationOptionClick() {
		locationOption.click();
	}
	
	public void clickRent() {
		rentBtn.click();
	}

	public void enterData(String place) {
		searchInput.sendKeys(place);
		
//		searchInput.sendKeys(Keys.ENTER);
	}
	
	public void clickOption() {
		selectOption.click();
	}
	
	public void clickSearch() {
		searchButton.click();
	}

}

```
BaseSteps

```
package tests;
import java.util.HashMap;
import java.util.Map;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;

public class BaseSteps {

    public static WebDriver driver;

    public static void setup() {

        ChromeOptions options = new ChromeOptions();

        Map<String, Object> prefs = new HashMap<>();

        // ✅ Block notifications permanently
        prefs.put("profile.default_content_setting_values.notifications", 2);

        // ✅ Extra recommended blocks (optional)
        prefs.put("profile.default_content_setting_values.geolocation", 2);
        prefs.put("profile.default_content_setting_values.media_stream", 2);

        options.setExperimentalOption("prefs", prefs);

        // ✅ Additional hard-block
        options.addArguments("--disable-notifications");
        options.addArguments("--disable-popup-blocking");

        driver = new ChromeDriver(options);
        driver.manage().window().maximize();
    }

    public static void teardown() {
        if (driver != null) {
            driver.quit();
        }
    }

}
```





