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

