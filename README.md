# Automating reCAPTCHA Solving with Puppeteer A Step by Step Guide
<p>Introduction: In this guide, I explore the automation of reCAPTCHA solving in web scraping and testing scenarios using Puppeteer, a Node.js tool designed for browser automation. My focus is on the practical use of the <code>puppeteer-extra-plugin-stealth</code> plugin to seamlessly navigate through reCAPTCHA challenges.</p>

> Note: I take <a href="https://2captcha.com/p/puppeteer-captcha-solver">this Guide</a> here.

<p>Understanding Puppeteer: Puppeteer is a browser automation tool written in Node.js, offering the unique feature of operating in headless mode, making it less detectable. This capability is essential for web scraping and automated testing, where being identified as a bot can obstruct access to web resources​​.</p>

<ul>
  <li>A captcha solving service, such as 2captcha.com.</li>
  <li>Puppeteer, the core automation tool.</li>
  <li>puppeteer-extra, a wrapper to enhance Puppeteer.</li>
  <li>puppeteer-extra-plugin-stealth, an add-on to disguise automation traces​​.</li>
</ul>

<p>Installation: Start by installing Puppeteer and the aforementioned packages using npm:</p>

```javascript
npm i puppeteer puppeteer-extra puppeteer-extra-plugin-stealth
```

<p>This sets the groundwork for our automation setup​​.</p>

<p>Configuring the Extension: Configure the captcha-solving extension by downloading and unzipping it into your project directory. Key settings include auto-solving specific captcha types and proxy support, which can be adjusted in the <code>/common/config.js</code> file. Ensure that the <code>autoSolveRecaptchaV2</code>  is set to true for recaptcha V2​​.</p>

<p>API Key Consideration: Include your API key in quotes in the configuration file to avoid script errors​​.</p>

<p>Additionally, to streamline the process, disable the opening of the extension’s settings page post-installation. This can be done by removing specific lines in the /manifest.json file, which otherwise would prompt the settings page to open automatically​​.</p>

```javascript
  "options_ui": {
    "page": "options/options.html",
    "open_in_tab": true
},
</per>
<p>Browser Automation Setup: Incorporate the stealth plugin into Puppeteer’s initialization to conceal the automation. This is crucial for bypassing detection mechanisms that websites might employ​​.</p>
<per>
 const puppeteer = require('puppeteer-extra');
const StealthPlugin = require('puppeteer-extra-plugin-stealth');
const { executablePath } = require('puppeteer'); 

(async () => {
  const pathToExtension = require('path').join(__dirname, '2captcha-solver');
  puppeteer.use(StealthPlugin())
  const browser = await puppeteer.launch({
    headless: false,
    args: [
      `--disable-extensions-except=${pathToExtension}`,
      `--load-extension=${pathToExtension}`,
    ],
    executablePath: executablePath()
  });

  const [page] = await browser.pages()
})(); 
```

<p>Navigating and Solving Captcha: Using Puppeteer’s <code>page.goto()</code>  function, navigate to the page with the captcha. Manually or automatically trigger the captcha-solving process. In this example, we manually initiate the process by waiting for the <code>.captcha-solver</code> button to appear and then clicking it​​.</p>

<p>Monitoring the Solution: Monitor the state of captcha solving through the data-state attribute of the ‘.captcha-solver’ button. The attribute changes from ‘ready’ to ‘solving’ and finally to ‘solved’, indicating the successful resolution of the captcha​​.</p>

```javascript
  // go to the specified address
await page.goto('https://2captcha.com/demo/recaptcha-v2') 

// wait until the element with the CSS selector ".captcha-solver" appears
await page.waitForSelector('.captcha-solver')
// click on the element with the specified selector
await page.click('.captcha-solver')
```

<p>Final Steps: Upon solving the captcha, perform the necessary actions on the page. In this example, we click a ‘Check’ button to verify the correctness of the solved captcha​​.</p>

```javascript
  //  By default, waitForSelector waits for 30 seconds, but this time is usually not enough, so we specify the timeout value manually as the second parameter. The timeout value is specified in "ms".
await page.waitForSelector(`.captcha-solver[data-state="solved"]`, {timeout: 180000})
```

<h2>Ready-to-Use File Download:</h2>

<p>For convenience, I’ve provided a ready-to-use file that includes all the necessary configurations. This file can be downloaded using the link below. Remember, after downloading and unzipping this file, you’ll need to add the solver folder (discussed earlier) into it. This step ensures that all components are in place and the setup is ready for immediate use.</p>

(https://github.com/2captcha/2captcha-solver-in-puppeteer)
<p>Conclusion: This guide demonstrates how to effectively automate reCAPTCHA solving in Puppeteer, providing a significant advantage in web scraping and automated testing scenarios. It’s important to use these techniques responsibly and ethically.</p>


