# SENSEA SPA Customer Survey

A comprehensive customer feedback survey for SENSEA SPA designed to gather insights for planning SENSEA 2025.

## Survey Structure

The survey includes 7 main questions plus opening and closing screens:

1. **Opening Screen** - Welcome message to top 100 SENSEA guests
2. **Question 1: Spa Day Pass Pricing Review** - Feedback on 4 pricing options
3. **Question 2: New Membership Offering Test** - Interest in Super Pass membership
4. **Question 3: Social and Ritual Events** - Feedback on new event offerings
5. **Question 4: Spa Opening Hours Review** - Input on off-peak hours
6. **Question 5: Loyalty Program** - Feedback on "Every 10th visit is on us" program
7. **Question 6: Treatments and Additional Offerings** - Treatment pricing and suggestions
8. **Question 7: Areas for Improvement** - General improvement suggestions
9. **Final Thoughts** - Closing feedback and thank you message

## Design

- **Background Color**: White (#FFFFFF)
- **Accent/Navigation Color**: Cyan (#00ccdf)
- **Progress Bar**: Shows completion percentage with #00ccdf fill
- **Responsive**: Works on desktop and mobile devices

## Google Sheets Integration

To connect the survey to Google Sheets:

1. **Create a Google Sheet** for collecting responses

2. **Create a Google Apps Script**:
   - In your Google Sheet, go to Extensions > Apps Script
   - Replace the default code with:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    
    // Get headers (first row)
    var headers = sheet.getRange(1, 1, 1, sheet.getLastColumn()).getValues()[0];
    
    // If sheet is empty, add headers
    if (headers.length === 0 || headers[0] === '') {
      var params = Object.keys(e.parameter);
      sheet.getRange(1, 1, 1, params.length).setValues([params]);
      headers = params;
    }
    
    // Create new row with data
    var newRow = headers.map(function(header) {
      return e.parameter[header] || '';
    });
    
    sheet.appendRow(newRow);
    
    return ContentService.createTextOutput(JSON.stringify({
      'result': 'success',
      'row': sheet.getLastRow()
    })).setMimeType(ContentService.MimeType.JSON);
    
  } catch(error) {
    return ContentService.createTextOutput(JSON.stringify({
      'result': 'error',
      'error': error.toString()
    })).setMimeType(ContentService.MimeType.JSON);
  }
}
```

3. **Deploy the Script**:
   - Click Deploy > New deployment
   - Select type: Web app
   - Execute as: Me
   - Who has access: Anyone
   - Click Deploy
   - Copy the Web app URL

4. **Update the Survey**:
   - Open `index.html`
   - Find the line: `const ENDPOINT_URL = 'https://script.google.com/macros/s/YOUR_GOOGLE_SHEETS_SCRIPT_ID/exec';`
   - Replace with your actual script URL

## Data Collected

The survey collects the following data:

### Question 1 - Pricing
- `q1_weekday` - Weekday pass fairness
- `q1_weekend` - Weekend pass fairness
- `q1_morning` - Morning pass fairness
- `q1_twilight` - Twilight pass fairness
- `q1_comment` - Open comment on pricing

### Question 2 - Membership
- `q2_interest` - Interest level
- `q2_price` - Price fairness
- `q2_feedback` - Feedback/suggestions
- `q2_trial` - Trial membership interest

### Question 3 - Events
- `q3_interest` - Interest level
- `q3_price` - Price fairness
- `q3_feedback` - Event feedback

### Question 4 - Hours
- `q4_usage` - After 5pm usage frequency
- `q4_impact` - Impact of hour changes
- `q4_feedback` - Hours feedback

### Question 5 - Loyalty
- `q5_appeal` - Program appeal
- `q5_feedback` - Program feedback

### Question 6 - Treatments
- `q6_single` - Single massage pricing
- `q6_couple` - Couple massage pricing
- `q6_current` - Thoughts on current treatments
- `q6_improve` - Improvement suggestions
- `q6_new` - New treatment ideas

### Question 7 - Improvement
- `q7_improvement` - General improvement areas

### Final
- `final_thoughts` - Final feedback
- `timestamp` - Submission timestamp
- `screenWhenSubmitted` - Screen number at submission

## Testing

To test the survey locally:
1. Open `index.html` in a web browser
2. Navigate through all questions
3. Submit the survey
4. Check browser console for data being sent

## Browser Compatibility

The survey is compatible with:
- Chrome/Edge (latest)
- Firefox (latest)
- Safari (latest)
- Mobile browsers (iOS Safari, Chrome Mobile)

## License

Copyright © 2024 SENSEA SPA. All rights reserved.
