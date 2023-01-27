# Creating chrome/firefox extension

### Preamble:

Before writing this post I never looked at creating an extension that can be used on both firefox and chrome browsers. Last week I needed to extract the colour code from an element on a website so I wrote this code that does the job:

```javascript
// Get the element you want to extract the color from
const element = document.querySelector("#my-element");

// Get the computed style of the element
const style = window.getComputedStyle(element);

// Extract the color property from the style object
const color = style.getPropertyValue("color");

console.log(color);
```

This code assumes that the element you want to extract the colour from has an ID of "my-element", and that it is using the "colour" property in its CSS to define the text colour. You can use the same approach to extract the background colour or any other CSS property.

```javascript
const bgColor = style.getPropertyValue("background-color");
console.log(bgColor);
```

You can also use this library named `color-thief` to extract colour from any image on the website:

```javascript
const colorThief = new ColorThief();
const color = colorThief.getColor("image.jpg");
console.log(color);
```

You can also do more with this library such as getting the colour palette from an image and much more. Here is the [*link*](https://lokeshdhakar.com/projects/color-thief/) to color-theif's documentation.

Next, I thought I should try it by creating an extension that works with chrome. It's very simple and easy to create an extension although it could become complex very easily if you try to add more functionality. There must be a few libraries that already do this but I am just writing as an example of a very basic chrome extension.

### How to create a chrome extension:

To create a Chrome extension, you will need to create a manifest file and include your JavaScript code in a separate file. The manifest file, named `manifest.json`, is a JSON file that tells Chrome about the extension's properties and permissions.

Here is an example of a basic manifest file for your extension:

```javascript
{
  "manifest_version": 1,
  "name": "Color Extractor",
  "version": "1.0",
  "permissions": ["activeTab", "storage","webNavigation"],
  "background": {
    "scripts": ["background.js"]
  },
  "content_scripts": [
    {
      "matches": ["<all_urls>"],
      "js": ["color-extractor.js"]
    }
  ],
  "browser_action": {
    "default_popup": "popup.html"
  },
  "options_page": "options.html"
}
```

This manifest file tells Chrome that your extension is named "Color Extractor", has a version of "1.0", and has permission to access the active tab and storage. It also tells Chrome to run the JavaScript code in a file named "color-extractor.js" on all URLs. It tells chrome to run a background script `background.js` and to inject `color-extractor.js` script on all URLs. It also tells chrome to open a `popup.html` when the extension icon is clicked and options page to be `options.html`.

Here is an example of the `color-extractor.js` file:

```javascript
const elements = document.querySelectorAll("*");
elements.forEach(function(element) {
    const style = window.getComputedStyle(element);
    const color = style.getPropertyValue("color");
    const bgColor = style.getPropertyValue("background-color");
    console.log("color:", color, " bgColor:", bgColor);
});
```

This script selects all the elements on the page and extracts the colour and background-color of the elements, you can add more functionality and styling to it.

Here is an example of the `background.js` file:

```javascript
chrome.webNavigation.onCompleted.addListener(function(details) {
    chrome.tabs.executeScript(details.tabId, {
        file: "color-extractor.js"
    });
});
```

This script listens to the `webNavigation` event and when a page is loaded completely it will execute the `color-extractor.js` script.

And the last file is `popup.html` and `options.html` which are the user interface for your extension, you can use any frontend library or framework to create them or you can use plain html/css/javascript.

Here is an example of a simple `popup.html` file that displays the extracted colour and background colour of the elements on the current page:

```xml
<!DOCTYPE html>
<html>
  <head>
    <title>Color Extractor</title>
    <style>
      .container {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        align-items: center;
        height: 100%;
      }
      .color-box {
        width: 50px;
        height: 50px;
        margin: 5px;
        border-radius: 5px;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="color-box" id="text-color"></div>
      <div class="color-box" id="bg-color"></div>
    </div>

    <script>
      chrome.tabs.query({ active: true, currentWindow: true }, function(tabs) {
        chrome.tabs.sendMessage(tabs[0].id, { type: "getColors" }, function(response) {
          document.getElementById("text-color").style.backgroundColor = response.textColor;
          document.getElementById("bg-color").style.backgroundColor = response.bgColor;
        });
      });
    </script>
  </body>
</html>
```

This file creates two divs with class 'color-box' and id 'text-color' and 'bg-color' respectively, it will use the background color of these divs to show the text and background color of the element on the current page. It uses `chrome.tabs.query` and `chrome.tabs.sendMessage` to communicate with the content script and get the color and background color.

And here is an example of the `options.html` file that allows the user to change the color of the text and background color of the elements on the website:

```xml
<!DOCTYPE html>
<html>
  <head>
    <title>Color Extractor Options</title>
  </head>
  <body>
    <h1>Color Extractor Options</h1>
    <form>
      <label for="text-color">Text Color:</label>
      <input type="color" id="text-color" name="text-color" value="#000000">
      <br>
      <label for="bg-color">Background Color:</label>
      <input type="color" id="bg-color" name="bg-color" value="#ffffff">
      <br>
      <button type="submit">Save</button>
    </form>

    <script>
      // Get current options from storage
      chrome.storage.local.get(["textColor", "bgColor"], function(options) {
        document.getElementById("text-color").
```

Please keep in mind that these are just examples and you will need to customize them to suit your needs. You will need to package this manifest file and your JavaScript code into a .zip file and then upload it to the chrome webstore or install it manually on your browser. Also, it is important to note that the extension should be tested before publishing.

### Implementing the same for the Firefox browser:

The process of creating a Firefox extension is similar to creating a Chrome extension, with a few key differences.

1. Firefox uses the `moz-extension` protocol instead of `chrome-extension`. So, you'll need to change all instances of `chrome` to `browser` in your JavaScript files.
    
2. Firefox uses the `browser` object instead of `chrome`, so you'll need to change all instances of `chrome` to `browser` in your JavaScript files.
    
3. Firefox uses `storage.local` API instead of `chrome.storage.local`.
    
4. Firefox uses the `browser.tabs.query` and `browser.tabs.sendMessage` APIs instead of `chrome.tabs.query` and `chrome.tabs.sendMessage`.
    
5. Firefox uses the `manifest.json` file to define the extension's metadata and permissions, similar to the `manifest.json` file in a Chrome extension.
    

So you'll need to change the code accordingly. You can check the documentation of Firefox extension development for more information [**https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions**](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions)**.**

Good luck :)