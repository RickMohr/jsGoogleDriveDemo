# jsGoogleDriveDemo
Let web app users save and open files in Google Drive using API v3 -- authorize, upload, get, update, and use the Google file picker.

## Intro
I wanted to let users of my web app save and open files in Google Drive, but it took several days of research to get all the details right. 
Google documentation for Drive API v3 is light on examples, and doesn’t cover the JavaScript `gapi` interface.
Internet search results for using the Drive API with JavaScript are often v2-specific or are for nodejs, which works differently than JavaScript in a browser.
And testing on your local machine requires some unexpected setup.
This is the demo I wish I had.

## Google Cloud setup
1. Use [this wizard](https://console.cloud.google.com/flows/enableapi?apiid=drive) to create a project in the Google Developers Console. Notes:
    * Specify "User Data" (not "Application Data")
    * Specify scope `https://www.googleapis.com/auth/drive.file`
    * Specify application type “Web Application”
    * For “Authorized JavaScript origins” add `http://lvh.me`
1. On the [credentials](https://console.cloud.google.com/apis/credentials) page, click `+ Add Credentials` and choose "API Key"
1. On the [OAuth consent screen](https://console.cloud.google.com/apis/consent) page, click `+ Add Users` and enter your Google account email address
1. On the [Enable APIs](https://console.cloud.google.com/apis/library) page, search and enable the “Google Picker API”
1. Verify that the [dashboard](https://console.cloud.google.com/apis/dashboard) table includes “Google Drive API” and “Google Picker API”
1. In `index.html`, replace `apiKey`, `clientId`, and `projectNumber` as directed in the code comments

## Run
1. In a shell, cd to the repo folder and [start a web server](https://developers.google.com/drive/api/v3/quickstart/js#step_2_run_the_sample), but use **port 80** instead of 8000. The Google APIs won’t work on other ports.
1. Point your browser to http://lvh.me/index.html (`lvh.me` resolves to `127.0.0.1`, aka `localhost`, but the Google API calls won't work if you use those directly)
1. Click the buttons in order:
    * `Authorize` connects to your Google Drive
        * When it says "Google hasn’t verified this app", click `Continue`, check the box for "See, edit, create, and delete only the specific Google Drive files you use with this app", and click `Continue`
    * `Upload` creates a file called `test.json` with mime type `application/my.app` containing `{"hello":"world"}`
    * `Get` retrieves the contents of that file
    * `Update` updates the file to contain `{"hello":"universe"}`
    * `Pick` opens the Google Drive Picker - select the file you just uploaded
3. Watch the console messages. You should see:
```
Authorized. Token: ya29.a0ARrdaM9AcSmAJcmlwyfnTlZ1KGNWhSkSQqaSF7UJwI_nsGqZft7V5fqyNN39jdT5luQZL0evbQtwy-5etF6FA0isaIF_7yN91UX77GHGhonvgJOYzWWbpL3PHbsRs73v3GxdG8yrG9WrTMQ726MORtHJ_P9g3Q

Uploaded. Result:
{
  "kind": "drive#file",
  "id": "1XJUM6NVh94LpM6ILAhXGu6zYz1IFFOSJ",
  "name": "test.json",
  "mimeType": "application/my.app"
}

Fetched. Result: {"hello":"world"}

Updated. Result:
{
  "kind": "drive#file",
  "id": "1XJUM6NVh94LpM6ILAhXGu6zYz1IFFOSJ",
  "name": "test.json",
  "mimeType": "application/my.app"
}

Picked. FileId: 1XJUM6NVh94LpM6ILAhXGu6zYz1IFFOSJ

Fetched. Result: {"hello":"universe"}
```

## Notes
Some posts suggest using a mime type of `application/vnd.google-apps.drive-sdk`.  For me trying to fetch files with that mime type failed with the error "Only files with binary content can be downloaded. Use Export with Docs Editors files."
