=====================================
CELURES WEBSITE — SETUP GUIDE
=====================================
Written for someone with no coding experience.
Follow each step carefully, one at a time.

=====================================
STEP 1 — OPEN YOUR WEBSITE FILES
=====================================
You have received a folder with 3 files:
  • index.html       ← your main landing page
  • thankyou.html    ← the thank you page
  • README.txt       ← this guide

To test your website right now:
1. Find the file called "index.html"
2. Double-click it
3. It will open in your browser (Chrome, Firefox, etc.)
4. You can click through, test the perfume selection, fill the form, and click Confirm Order
5. It will take you to thankyou.html — this means it's working!

NOTE: At this stage, orders are NOT being saved anywhere yet.
To save orders to Google Sheets, follow Step 2 below.

=====================================
STEP 2 — CONNECT GOOGLE SHEETS + GMAIL
=====================================
This is a one-time setup. Takes about 10 minutes.

--- PART A: Create your Google Sheet ---

1. Go to: https://sheets.google.com
2. Click the big + button to create a new spreadsheet
3. Name it "Celures Orders"
4. In Row 1, type these headers in columns A through H:
   A1: Timestamp
   B1: Name
   C1: Phone
   D1: District
   E1: Address
   F1: Perfume 1
   G1: Perfume 2
   H1: Notes
5. Leave everything else blank — orders will fill in automatically

--- PART B: Create the Google Script ---

1. In your Google Sheet, click the top menu: Extensions → Apps Script
2. A new tab will open with a code editor
3. Delete everything that is already there
4. Copy and paste the code below (everything between the === lines):

=== COPY FROM HERE ===

function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data = JSON.parse(e.postData.contents);

    sheet.appendRow([
      data.timestamp || new Date().toLocaleString(),
      data.name || '',
      data.phone || '',
      data.district || '',
      data.address || '',
      data.perfume1 || '',
      data.perfume2 || '',
      '30ml x2 — ৳1,390'
    ]);

    // Send email notification to yourself
    var emailBody = 
      "New Celures Order!\n\n" +
      "Name: " + data.name + "\n" +
      "Phone: " + data.phone + "\n" +
      "District: " + data.district + "\n" +
      "Address: " + data.address + "\n\n" +
      "Perfume 1: " + data.perfume1 + "\n" +
      "Perfume 2: " + data.perfume2 + "\n\n" +
      "Amount: ৳1,390 + delivery\n" +
      "Time: " + data.timestamp;

    MailApp.sendEmail({
      to: "YOUR_EMAIL@gmail.com",   // <-- REPLACE with your Gmail address
      subject: "New Order: " + data.name + " (" + data.district + ")",
      body: emailBody
    });

    return ContentService
      .createTextOutput(JSON.stringify({status: "success"}))
      .setMimeType(ContentService.MimeType.JSON);

  } catch(err) {
    return ContentService
      .createTextOutput(JSON.stringify({status: "error", message: err.toString()}))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

=== COPY UNTIL HERE ===

5. IMPORTANT: In the pasted code, find this line:
      to: "YOUR_EMAIL@gmail.com",
   Replace YOUR_EMAIL@gmail.com with your actual Gmail address

6. Click the floppy disk icon (Save) or press Ctrl+S
7. Name your project "Celures Orders" when asked

--- PART C: Deploy the Script ---

1. Click the blue "Deploy" button (top right)
2. Click "New deployment"
3. Click the gear icon next to "Type" and select "Web app"
4. Fill in the settings:
   - Description: Celures Orders
   - Execute as: Me
   - Who has access: Anyone
5. Click "Deploy"
6. Google will ask you to authorize — click "Authorize access"
7. Choose your Gmail account
8. You may see a warning "Google hasn't verified this app" — click "Advanced" then "Go to Celures Orders (unsafe)" — this is safe, it's your own script
9. Click "Allow"
10. After deploying, you will see a "Web app URL" — it looks like:
    https://script.google.com/macros/s/XXXXXXXXXX/exec
11. COPY that entire URL

--- PART D: Connect the URL to your website ---

1. Open the file "index.html" in VS Code
   (Right-click the file → Open With → VS Code)
2. Press Ctrl+F to open the search bar
3. Search for this exact text:
   YOUR_GOOGLE_APPS_SCRIPT_URL_HERE
4. Replace it with the URL you copied in Part C
5. Save the file (Ctrl+S)

That's it! Now test it:
- Open index.html in your browser
- Fill the form and submit
- Check your Google Sheet — a new row should appear
- Check your Gmail — you should get an email notification

=====================================
STEP 3 — ADD YOUR PERFUME IMAGES
=====================================
When you have photos of your perfume bottles:

1. Create a folder called "images" in the same folder as index.html
2. Name your images exactly like this:
   1.jpg  ← Nautica Voyage
   2.jpg  ← Acqua di Giò Profumo
   3.jpg  ← Dior Sauvage Elixir
   4.jpg  ← Versace Eros
   5.jpg  ← Club de Nuit Intense
   6.jpg  ← Davidoff Cool Water
   7.jpg  ← Chanel Allure Homme Sport
   8.jpg  ← YSL Y
   9.jpg  ← Lacoste Blanc
   10.jpg ← Rasasi Hawas Kobra
   11.jpg ← Dunhill Blue
   12.jpg ← Invictus
3. Put all image files inside the "images" folder
4. Refresh your browser — images will appear automatically

=====================================
STEP 4 — ADD YOUR BRAND VIDEO
=====================================
When you have a video ready:

1. Put your video file (e.g. video.mp4) in the same folder as index.html
2. Open index.html in VS Code
3. Search for:   src=""
   (inside the <video> tag near the top)
4. Replace it with:   src="video.mp4"
5. Also search for:   style="display:none"   (on the <video> line)
   And delete that part
6. Save the file

=====================================
STEP 5 — PUT YOUR WEBSITE ONLINE
=====================================
Right now your website only works on your own computer.
To make it available to customers online, you need web hosting.

Easy free option: Netlify
1. Go to: https://netlify.com
2. Sign up with your Gmail
3. Drag and drop your entire website FOLDER onto their website
4. They give you a free link like: https://yoursite.netlify.app
5. Share that link with your customers

Paid option (if you have a domain like celures.com):
Ask me to help you set it up — just say "help me put the website online with my domain"

=====================================
TROUBLESHOOTING
=====================================

Q: The website opens but looks broken?
A: Make sure all files are in the SAME folder. Don't move them separately.

Q: Clicking Confirm Order doesn't do anything?
A: Make sure you selected 2 perfumes AND filled all form fields.

Q: Orders aren't going to my Google Sheet?
A: Make sure you replaced YOUR_GOOGLE_APPS_SCRIPT_URL_HERE with the real URL. Also make sure you re-deployed after any changes to the script.

Q: I'm not getting email notifications?
A: Check your Gmail spam folder. Also make sure you replaced YOUR_EMAIL@gmail.com in the script.

=====================================
NEED HELP?
=====================================
Just ask Claude! Copy any error message you see and paste it in the chat.
Always share your current index.html when asking for changes.

=====================================
