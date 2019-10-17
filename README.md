Change background Color - chrome extension
--------------------------------------------

## Steps from scratch

`mkdir change-background-chrome-extension`

`cd change-background-chrome-extension`

`git init`

`echo .idea >> .gitignore`

`touch manifest.json`

#### Add the following into manifest.json
```json
{
  "name": "Change Background Color",
  "version": "1.0",
  "description": "This chrome extension will help you to change the website's background color.",
  "manifest_version": 2,
  "permissions": [
    "storage"
  ],
  "background": {
    "scripts": [
      "scripts/background.js"
    ],
    "persistent": false
  }
}
```

`git add .`

`gc -m "Add manifest json"`


`mkdir scripts`

`touch scripts/background.js`


#### Add the following into background.js

```js
chrome.runtime.onInstalled.addListener(function () {
    chrome.storage.sync.set({ color: '#3aa757' }, function () {
        console.log("The color is green.");
    });
});
```

`git status`

`git add .`

`gc -m "Add background js with storage"`


`git remote add origin git@github.com:sujithps/change-background-chrome-extension.git`

`git push -u origin master`

`git status`

`touch README.md` 

// Add readme descriptions and continue editing this file for any change.

### Add a popup to the extension

`touch popup.html`

Add the following to popup.html

```html
  <!DOCTYPE html>
  <html>
    <head>
      <style>
        button {
          height: 30px;
          width: 30px;
          outline: none;
        }
      </style>
    </head>
    <body>
      <button id="change-color"></button>
    </body>
  </html>
```

Add the following into manifest.json

```json
"page_action": {
      "default_popup": "popup.html"
 },
```

`mkdir images`

Copy icons(with different resolutions) to images folder.

Add the following to manifest.json under `page_action` (change filenames accordingly)

```json
     "default_icon": {
        "16": "images/get_started16.png",
        "32": "images/get_started32.png",
        "48": "images/get_started48.png",
        "128": "images/get_started128.png"
      }
```


Extensions also display images on the extension management page, the permissions warning, and favicon. 
These images are designated in the manifest under icons.

```json
    "icons": {
      "16": "images/get_started16.png",
      "32": "images/get_started32.png",
      "48": "images/get_started48.png",
      "128": "images/get_started128.png"
    }
```

`page_action` is declared in the manifest, it is up to the extension to tell the browser when the user can interact with popup.html.


Add declared rules to the background script with the declarativeContent API within the runtime.onInstalled listener event.


```js

chrome.declarativeContent.onPageChanged.removeRules(undefined, function() {
      chrome.declarativeContent.onPageChanged.addRules([{
        conditions: [new chrome.declarativeContent.PageStateMatcher({
          pageUrl: {hostEquals: 'developer.chrome.com'},
        })
        ],
            actions: [new chrome.declarativeContent.ShowPageAction()]
      }]);
    });
```


The extension will need permission to access the declarativeContent API in its manifest.


```js
"permissions": ["declarativeContent", "storage"]
```



`touch popup.js`

### Add the following logic to popup.js

```js
let changeColor = document.getElementById('change-color');

  chrome.storage.sync.get('color', function(data) {
    changeColor.style.backgroundColor = data.color;
    changeColor.setAttribute('value', data.color);
  });
```


