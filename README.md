# ProjectBase

package core;

import org.openqa.selenium.*;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.remote.RemoteWebDriver;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

import java.io.FileInputStream;
import java.util.Properties;
import java.util.logging.*;

public class Chrome {
	static Properties p = new Properties();
	static WebDriver driver;

	public static void main(String[] args) throws Exception {
		p.load(new FileInputStream("/Users/user1/eclipse-workspace/pj41/input.properties"));
		Logger.getLogger("").setLevel(Level.OFF);
		String driverPath = "";

		if (System.getProperty("os.name").toUpperCase().contains("MAC"))
			driverPath = "/usr/local/bin/chromedriver";
		
		else if (System.getProperty("os.name").toUpperCase().contains("WINDOWS"))
			driverPath = "c:\\windows\\geckodriver.exe";
		else throw new IllegalArgumentException("Unknown OS");

		System.setProperty("webdriver.gecko.driver", driverPath);
		System.out.println("Browser: Chrome");

		System.setProperty("webdriver.gecko.driver", "/usr/local/bin/chromedriver");
		System.setProperty("webdriver.chrome.silentOutput", "true"); // Chrome
		ChromeOptions option = new ChromeOptions();                  // Chrome
		option.addArguments("disable-infobars");                     // Chrome
		option.addArguments("--disable-notifications");              // Chrome
		System.out.println("===================================================");
		driver = new ChromeDriver();
		Capabilities caps = (((RemoteWebDriver) driver).getCapabilities());
		System.out.println("OS: " + System.getProperty("os.name"));
		System.out.println("Browser: " + caps.getBrowserName().substring(0, 1).toUpperCase() + caps.getBrowserName().substring(1).toLowerCase()  + " " + caps.getVersion());
		driver.get(p.getProperty("url"));
		System.out.println("Page URI: " + driver.getCurrentUrl());
		System.out.println("Page Title: " + driver.getTitle());
		driver.manage().window().maximize();
		
		WebDriverWait wait = new WebDriverWait(driver, 15);
		
wait.until(ExpectedConditions.presenceOfElementLocated(By.id(p.getProperty("fname_id"))))
.sendKeys(p.getProperty("fname_value"));
wait.until(ExpectedConditions.presenceOfElementLocated(By.id(p.getProperty("lname_id"))))
.sendKeys(p.getProperty("lname_value"));
wait.until(ExpectedConditions.presenceOfElementLocated(By.id(p.getProperty("email_id"))))
.sendKeys(p.getProperty("email_value"));
wait.until(ExpectedConditions.presenceOfElementLocated(By.id(p.getProperty("phone_id"))))
.sendKeys(p.getProperty("phone_value"));
wait.until(ExpectedConditions.presenceOfElementLocated(By.id(p.getProperty("submit_id"))))
.click();

System.out.println("===================================================");
System.out.println("Page URI: "   + driver.getCurrentUrl());
System.out.println("Page Title: " + driver.getTitle());
System.out.println("First Name: " + driver.findElement(By.id(p.getProperty("fname_id"))).getText());
System.out.println("Last Name: "  + driver.findElement(By.id(p.getProperty("lname_id"))).getText());
System.out.println("Email: "      + driver.findElement(By.id(p.getProperty("email_id"))).getText());
System.out.println("Phone: "      + driver.findElement(By.id(p.getProperty("phone_id"))).getText());
		driver.quit();
	}
}
