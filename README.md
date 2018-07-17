# C-Training
Developer aspiration
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
