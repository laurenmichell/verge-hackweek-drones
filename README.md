# Overview
This is a temporary bandaid tool created by Lauren to help editorial generate newsletters (HTML and plain text) via a Google Spreadsheet. 

# Dependencies
 - jQuery: http://jquery.com/
 - tabletop.js: https://github.com/jsoma/tabletop
 - handlebars.js: http://handlebarsjs.com/

# Set up

## 1. Create the Google spreadsheet
Create a Google doc that will serve as the lightweight database to power the content of the email newsletter. Each column of the spreadsheet will represent a variable that the template loops over to populate the newsletter. 

The Verge's newsletter is set up with the following sheets and columns:

### Sheet: editorsnote

This sheet powers the top section of the newsletter, above the first ad. If the editor's note is present, it will display. If the editor's not is empty, the markup will not display. It is also the sheet that controls the content of the lead story. 

Columns:

 - **editorsnote:** If populated with *any value*, the editor's note markup displays; if empty, editor's note markup does not display.
 - **leadhed:** Plain text for the headline of the lead story
 - **leadlink:** URL for the lead story, which will wrap around the lead image and headline.
 - **leadimageurl:** Image URL for the lead image. Grabbed from Chorus. 
 - **leadsummary:** Blurb that goes with lead story.
 - **advertisementon:** If populated with *any value*, the markup for the top advertisement will display; if empty, markup will not display. 

### Sheet: featured stories

This sheet powers the middle section containing three big stories, one of which will occassionally be a Harmony sponsored post. 

Columns:

- **featuredhed:** Headlines for the featured stories go in this column. 
- **featuredurl:** URL for features stories go in this column. 
- **featuredimgurl:** URL for featured stories goes here. This is grabbed from Chorus. 
- **featuredblurb:** Hand-crafted blurb for featured stories goes here.
- **harmony:** If populated with *any value*, the corresponding story in that row will get a 'FROM OUR SPONSOR' label, a 'Powered by Vox Creative' byline and a right-aligned sponsored title bar. 

### Sheet: storylist

This powers the numbered list of headlines that appears in the lowermost section of the email newsletter. 

Columns:

- **storyhed:** Headline for the first story
- **storyurl:** URL for the story
- **storyblurb:** Blurb for the story
- **number:** Sailthru image URL for the numbered burst icon

### Sheet: advertiseon

This is a custom sheet for the bottom variable advertisement. 

Columns:

- **advertiseon:** If populated with *any* value, markup for displaying ad will render in markup. If empty, ad will not display. 

## 2. Publish spreadsheet. 

For tabletop/handlebars to access data from the spreadsheet, it needs to be publishing to the web. To make the sheet public, go to `File > Publish to the web > All sheets > Start publishing`. 

Copy the link to the published data, which you will use in `index.html`, `html.hml` and `plaintext.html` in the next step. 

# 3. Pages

## index.html
This page is what displays the preview of the newsletter and provides HTML and plain text markup for editorial to paste into sailthru.

## html.html
This page displays the encoded html, which will render decoded so it can be copied/pasted into sailthru.

## plaintext.html
This page displays the plain text, stripped-down version of the email. 

# Resources
- Yahoo Mail sucks: http://www.emailonacid.com/blog/details/C13/stop_yahoo_mail_from_rendering_your_media_queries
- Mailchimp inliner tool (useful for the html.html page): http://templates.mailchimp.com/resources/inline-css/
- Converting HTML entities (useful for the html.html page): http://htmlentities.net/


