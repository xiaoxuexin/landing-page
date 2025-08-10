# DatrixLab Landing Page

A modern, responsive landing page for DatrixLab's AI-powered data platform, featuring EmailJS integration for demo requests and Google Sheets for data collection.

## Features

- **Responsive Design**: Mobile-first approach with TailwindCSS
- **Email Integration**: EmailJS for sending demo request emails
- **Data Collection**: Google Sheets integration for storing user information
- **Local Storage**: Fallback database for offline functionality
- **Admin Panel**: Built-in tools for managing demo requests
- **Test Panel**: Development tools for testing integrations

## Setup Instructions

### 1. EmailJS Configuration

EmailJS is already configured with the following credentials:
- **Public Key**: `UGgP4tO8lvPhngpbK`
- **Service ID**: `service_ytqm5hs`
- **Template ID**: `template_itml2mm`

#### To set up your own EmailJS account:

1. **Sign up at [EmailJS](https://www.emailjs.com/)**
   - Free tier available (200 emails/month)
   - Paid plans for higher volume

2. **Create an Email Service**
   - Go to Email Services → Add New Service
   - Choose your email provider (Gmail, Outlook, etc.)
   - Follow the authentication steps

3. **Create an Email Template**
   - Go to Email Templates → Create New Template
   - Use variables: `{{from_name}}`, `{{from_email}}`, `{{company}}`, etc.
   - Save and note the Template ID

4. **Update the code with your credentials**
   ```javascript
   emailjs.init("YOUR_PUBLIC_KEY");
   // Update service_id and template_id in the form submission code
   ```

### 2. Google Sheets Integration

The page is configured to send data to a Google Apps Script web app:
- **Web App URL**: `https://script.google.com/macros/s/AKfycbwrg7jM9lQxAhkqB9L1WB9ge7aVmjTISproXo_1IOi95MKUtJb_1m0eDU2kX5i7DG4u6A/exec`

#### To set up your own Google Sheets integration:

1. **Create a Google Sheet**
   - Create a new Google Sheet
   - Add headers: `Timestamp`, `Full Name`, `Email`, `Company`, `Job Title`, `Use Case`, `Message`

2. **Create Google Apps Script**
   - Go to [Google Apps Script](https://script.google.com/)
   - Create a new project
   - Replace the default code with:

   ```javascript
   function doPost(e) {
     try {
       const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
       const data = e.parameter;
       
       const row = [
         new Date(),
         data.full_name,
         data.email,
         data.company,
         data.job_title,
         data.use_case,
         data.message
       ];
       
       sheet.appendRow(row);
       
       return ContentService
         .createTextOutput(JSON.stringify({ 'result': 'success' }))
         .setMimeType(ContentService.MimeType.JSON);
         
     } catch(error) {
       return ContentService
         .createTextOutput(JSON.stringify({ 'result': 'error', 'error': error.toString() }))
         .setMimeType(ContentService.MimeType.JSON);
     }
   }
   ```

3. **Deploy as Web App**
   - Click Deploy → New deployment
   - Choose "Web app"
   - Set access to "Anyone"
   - Copy the Web App URL
   - Update the URL in the HTML file

### 3. Testing the Integrations

#### Admin Panel (Top Right)
- **Test Connections**: Verify EmailJS and Google Sheets are working
- **View Requests**: See all demo requests stored locally
- **Sync to Sheets**: Manually sync local data to Google Sheets
- **Export CSV**: Download all requests as CSV file
- **Clear Data**: Remove all local data

#### Test Panel (Bottom Left - Development)
- **Show Test Panel**: Reveal testing tools
- **Test Email**: Send a test email via EmailJS
- **Test Sheets**: Send test data to Google Sheets
- **Hide**: Conceal the test panel

## Troubleshooting

### EmailJS Issues

1. **"EmailJS is not loaded"**
   - Check internet connection
   - Verify EmailJS CDN is accessible
   - Check browser console for errors

2. **"Service connection failed"**
   - Verify Service ID is correct
   - Check if email service is active
   - Ensure template exists and is published

3. **"Template not found"**
   - Verify Template ID is correct
   - Check if template is published
   - Ensure template variables match the code

### Google Sheets Issues

1. **"CORS error"**
   - This is normal with `mode: 'no-cors'`
   - Check if data appears in your Google Sheet
   - Verify the web app URL is correct

2. **"Script execution failed"**
   - Check Google Apps Script logs
   - Verify the script has permission to access the sheet
   - Ensure the sheet is shared with the script

3. **Data not appearing in sheets**
   - Check the script logs in Google Apps Script
   - Verify the sheet name and structure
   - Test the web app URL manually

### General Issues

1. **Form not submitting**
   - Check browser console for JavaScript errors
   - Verify all required fields are filled
   - Check if EmailJS is initialized

2. **Admin panel not visible**
   - Check if JavaScript is enabled
   - Look for console errors
   - Verify the admin panel code is loaded

## File Structure

```
├── index.html          # Main landing page with all functionality
├── styles.css          # Additional custom styles
├── script.js           # Additional JavaScript functions
└── README.md           # This file
```

## Browser Compatibility

- **Modern Browsers**: Chrome 80+, Firefox 75+, Safari 13+, Edge 80+
- **Mobile**: iOS Safari 13+, Chrome Mobile 80+
- **Features**: ES6+ JavaScript, CSS Grid, Flexbox

## Security Notes

- EmailJS public key is safe to expose in client-side code
- Google Apps Script web app URL is public by design
- Local storage data is only accessible to the user
- No sensitive data is transmitted without user consent

## Support

For technical issues:
1. Check the browser console for error messages
2. Use the admin panel to test connections
3. Verify your EmailJS and Google Sheets configurations
4. Check the troubleshooting section above

## License

This project is proprietary to DatrixLab. All rights reserved.
