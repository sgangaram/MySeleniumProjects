# MySeleniumProjects
Sample Selenium projects and research

package com.test.selenium;

import static org.junit.Assert.fail;

import java.util.concurrent.TimeUnit;

import org.junit.After;
import org.junit.Before;
import org.junit.Test;
import org.openqa.selenium.By;
import org.openqa.selenium.NoSuchElementException;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.firefox.FirefoxDriver;

public class GmailLoginAndLogout {

	private WebDriver driver;
	private WebElement usr_element, pwd_element;
	private String baseUrl;
	private StringBuffer verificationErrors = new StringBuffer();

	@Before
	public void setUp() throws Exception {
		System.setProperty("webdriver.gecko.driver","C:\\My data\\Software\\geckodriver-v0.14.0-win64\\geckodriver.exe");
		driver = new FirefoxDriver();
		driver.manage().timeouts().implicitlyWait(20, TimeUnit.SECONDS);
		baseUrl = "https://www.google.com/gmail/about/";
		// WebDriverWait wait = new WebDriverWait(driver, 30);
		// driver.manage().window().maximize();
	}

	/**
	 * @throws Exception
	 */
	@Test
	public void test() throws Exception {
		driver.navigate().to(baseUrl);
		driver.findElement(By.linkText("Sign In")).click();
		// wait.until(ExpectedConditions.presenceOfElementLocated(By.name("Email")));

		// Find and enter user id/Email id
		usr_element = driver.findElement(By.cssSelector("input[id=Email]"));
		driver.findElement(By.name("Email")).clear();
		usr_element.sendKeys(new String[] { "swathi.test678@gmail.com" });

		// Click the next button
		driver.findElement(By.id("next")).click();
		// wait.until(ExpectedConditions.presenceOfElementLocated(By.id("Passwd")));

		// Find and enter Password
		pwd_element = driver.findElement(By.id("Passwd"));
		driver.findElement(By.name("Passwd")).clear();
		pwd_element.sendKeys(new String[] { "Test@123" });

		// Click SignIn button
		driver.findElement(By.id("signIn")).click();
		// wait.until(ExpectedConditions.visibilityOfElementLocated(By.xpath("//span[@class='gb_9a gbii']")));

		// Sign out from Gmail account
		driver.findElement(By.xpath("//span[@class='gb_9a gbii']")).click();
		for (int second = 0;; second++) {
			if (second >= 60)
				fail("timeout");
			try {
				if (isElementPresent(By.xpath("//*[@id='gb_71']")))
					break;
			} catch (Exception e) {
			}
			Thread.sleep(1000);
		}
		driver.findElement(By.id("gb_71")).click();
		try {
			//Validate successful logout
			if (isElementPresent(By.linkText("Sign in with a different account")));
			System.out.println("Succefully logged out");
		} catch (Exception e) {
			System.out.println("failed to log out");
		}
	}

	private boolean isElementPresent(By by) {
		try {
			driver.findElement(by);
			return true;
		} catch (NoSuchElementException e) {
			return false;
		}
	}

	@After
	public void tearDown() throws Exception {
		driver.quit();
		String verificationErrorString = verificationErrors.toString();
		if (!"".equals(verificationErrorString)) {
			fail(verificationErrorString);
		}
	}

}

