# Wedding Invitation Website

This repository contains the source code for a responsive, minimalistic wedding invitation website for **Jamshidbek &amp; Kakhrabo**.  The site is written in plain HTML, CSS and JavaScript and is fully adaptive to both desktop and mobile devices.  All text is provided in the Kazakh language as requested.

In this updated version the design shifts away from the original white‑and‑gold palette toward a warmer, more contemporary look.  A rich brown accent colour is paired with two light parchment tones, while a soft pastel watercolour texture sits behind everything for depth.  The hero section now uses one of your own photos as a background with a gentle dark overlay so that the names and date remain readable.  The countdown, RSVP form and other elements have been re‑tinted to harmonise with the new palette.  The music control button has been styled to be more legible against the darker header.

## Features

* **Responsive design** – the layout adapts gracefully to different screen sizes.
* **Elegant typography and minimal styling** – using Google Fonts to achieve a refined look.
* **Warm, earthy colour palette** – a bold brown accent colour is paired with two light parchment tones.  A subtle pastel texture sits behind everything to add depth without distracting from the content.
* **Countdown timer** – counts down to 24 Қаңтар 2026 сағат 19:00.
* **Personal photos** – the hero uses your own couple photo as a backdrop.  Additional slots are provided in the photo section; simply replace `images/couple1.jpg` with your preferred pictures.
* **Music player** – a round “Музыка” button in the hero section toggles background music.  The file `audio/song.mp3` contains your chosen track; to change the music, simply replace this file.  For a romantic mood, Miras Zhugunusov’s love song **“Sen Turaly”** is a lovely option【225771903251596†L121-L124】.
* **Google Maps embed** – a simple map showing the venue location at *Түркістан қаласы, Ескі Иқан ауылы, “Азizbek” тойханасы*.  You can adjust the address in the `<iframe>` `src` URL if needed.
* **RSVP form** – collects guest names, attendance choice, number of attendees and an optional message.  The form can be connected to a Google Sheet to store responses.

## Deploying the website

1. **Download the files** – copy the contents of the `website` folder to your own computer or hosting environment.
2. **Replace placeholders** – the images in `images/floral.png` and `images/couple‑silhouette.png` are generic examples.  Swap these files for your own couple photos if desired.
3. **Edit text if necessary** – open `index.html` and update any wording, dates or addresses to suit your event.
4. **Host the site** – you can view the site locally by opening `index.html` in a web browser.  To make it publicly accessible, upload the folder to any static hosting provider (e.g. GitHub Pages, Netlify, Vercel or your own web server).

## Linking the RSVP form to Google Sheets

To save form submissions into a Google Sheet you can use a simple Google Apps Script.  Below are step‑by‑step instructions:

1. **Create a new Google Sheet** – go to [Google Sheets](https://docs.google.com/spreadsheets/) and create an empty spreadsheet.  Give it a meaningful name (e.g. `Jamshidbek_Kakhrabo_RSVP`).  In the first row of the sheet enter column headings such as `Timestamp`, `Name`, `Attendance`, `GuestCount`, `Message`.

2. **Open the Script Editor** – from the Google Sheets menu select **Extensions → Apps Script**.  Delete any code that might be there and paste the following script:

   ```javascript
   // Code.gs (Google Apps Script)
   function doPost(e) {
     // Parse the JSON request body
     var data = JSON.parse(e.postData.contents);
     var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Sheet1');
     // Append a new row with the current timestamp and the form data
     sheet.appendRow([
       new Date(),
       data.name,
       data.attendance,
       data.guestCount,
       data.message
     ]);
     // Return a success response
     return ContentService.createTextOutput('Success')
       .setMimeType(ContentService.MimeType.TEXT);
   }
   ```

   *If your sheet has a different name than `Sheet1`, update the call to `getSheetByName()` accordingly.*

3. **Save and deploy the script** – click the **Save** icon, then select **Deploy → New deployment**.  Choose **Web App** as the deployment type.  In the configuration dialog set:
   * **Description** – e.g. `RSVP form handler`.
   * **Execute as** – `Me`.
   * **Who has access** – `Anyone` (or `Anyone with the link`).

   Click **Deploy**.  The first time you deploy, Google will ask you to authorize the script to access the sheet; follow the prompts and grant the necessary permissions.

4. **Copy the web app URL** – after deployment you will receive a **Web app URL** that looks like `https://script.google.com/macros/s/AKfycbx.../exec`.  Copy this URL.

5. **Update your website** – open `script.js` and replace the placeholder string `'YOUR_GOOGLE_APPS_SCRIPT_URL'` with your copied Web App URL.  For example:

   ```javascript
   const scriptURL = 'https://script.google.com/macros/s/AKfycbx12345/exec';
   ```

6. **Test the form** – refresh your hosted website and submit a test RSVP.  After submission, check your Google Sheet; a new row with the submitted information should appear.

## Customization

* **Adjust the countdown date/time** – modify the `weddingDate` in `script.js` to reflect a different date or time if needed.
* **Change the map** – in `index.html`, update the query string inside the `<iframe>` `src` attribute to target a different venue.  You can construct a new embed URL by searching for your venue on Google Maps, choosing **Share → Embed a map** and copying the link.
* **Add or remove form fields** – you can extend the form markup in `index.html` and adjust the data object in `script.js` accordingly.  Don’t forget to update your Apps Script to handle any extra fields.

## License

The code provided here is intended for educational and personal use.  You are free to modify it to suit your needs.