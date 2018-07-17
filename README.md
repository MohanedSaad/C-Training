# C-Training
Developer aspiration

#Class to help login (OOP Method)
namespace PracticeAutomation1
{
    public class LoginOutHelper
    {
        protected IWebDriver _driver;
        protected LoginPage _loginPage;
        
        public void Login()
        {
            _loginPage.LoginMethod("Callcredit84", "K6dnnnnx");
        }

        public void Logout()
        {
            _loginPage.LogOutMethod();
            _driver.Close();
        }
    }
}
######################################
#PageObejects (LoginPage)
namespace PracticeAutomation1.PageObjects   
{
    public class LoginPage
    {
        private IWebDriver driver;
        //Constrcutor 
        public LoginPage(IWebDriver driver)
        {
            this.driver = driver;
        }
        
        //Objects  
        private IWebElement Username
        {
            get
            {
                return driver.FindElement(By.Id("Username"));
            }
        }

        private IWebElement Password
        {
            get
            {
                return driver.FindElement(By.Id("Password"));
            }
        }

        private IWebElement LoginButton
        {
            get
            {
                return driver.FindElement(By.XPath("//a[contains(text(),'Log in')]"));
            }
        }

        private IWebElement LogOutButton
        {
            get
            {
                return driver.FindElement(By.XPath("//a[contains(text(),'Log out')]"));
            }
        }

        private IWebElement Submit
        {
            get
            {
                return driver.FindElement(By.XPath("//input[@type='submit']"));
            }
        }
        //Methods
        public string LoginMethod(string username, string password)
        {
            LoginButton.Click();
            string LoginLandingPage = driver.Title;
            Username.SendKeys(username);
            Password.SendKeys(password);
            Submit.Click();
            return LoginLandingPage; 
            
        }

        public void LogOutMethod()
        {
            LogOutButton.Click(); 
        }
    }
}
################################################################

#PageObejects (HomePage)
namespace PracticeAutomation1.PageObjects   
{
    public class HomePage
    {
        //Constructor 
        IWebDriver driver; 
        public HomePage(IWebDriver driver)
        {
            this.driver = driver; 
        }

        //Method 
        public string GetHomePageTitle()
        {
            string HomePageTitle = driver.Title;
            return HomePageTitle; 
        }
    }
}
################################################################
NUnit Framework
namespace TestingFolder
{
    [TestFixture()]
    public class NUnitTest : LoginOutHelper
    {
        [SetUp]
        public void SetUp()
        {
            _driver = new ChromeDriver();
            _driver.Manage().Window.Maximize();
            _driver.Manage().Cookies.DeleteAllCookies();
            _driver.Navigate().GoToUrl("https://www.noddle.co.uk/");

            _loginPage = new LoginPage(_driver);
        }
        
        [TestCase]
        public void SimpleLogin()
        {
            Login();

            HomePage homePageObj = new HomePage(_driver);
            string assertLoginPage = homePageObj.GetHomePageTitle();
            Assert.AreEqual("Home - Noddle | Free For Life Credit Report And Credit Score", assertLoginPage);

            Logout();
        }
        
        [TestCase]
        public void SanityCheckTabs()
        {
            Login();

            MyCreditReportTab mycreditreporttabObj = new MyCreditReportTab(_driver);
            mycreditreporttabObj.SanityClicks();
            HomePage homePageObj = new HomePage(_driver);
            string assertmycreditreportPage = homePageObj.GetHomePageTitle();
            Assert.AreEqual("Credit report - Noddle | Free For Life Credit Report And Credit Score", assertmycreditreportPage);

            Logout();
        }
    }
}
