## Overview
This is a temporary bandaid tool created by Lauren to help editorial generate newsletters (HTML and plain text) via a Google Spreadsheet for use with Sailthru's email newsletter templates and campaigns. Editors/producers can add headlines, blurbs, article URLs and image URLs to cells in a spreadsheet, which then spits out JSON that gets parsed into plain html markup giving you (1) a preview of the newsletter (2) generated code for the HTML version of the newsletter and (3) plain text for the plain text version of the newsletter. 

The value in doing this is that until we have Chorus -> Sailthru integration or a custom email newsletter solution, all newsletters created will have to be done manually. To reduce the amount of human error involved in mucking around in markup or manipulating text via Sailthru's visual editor (which is also prone to creating html errors inadvertently), this tool removes editors and producers from the step of touching markup. 

This June 2014 implementation is meant to be a temporary solution until a more sustainable option can be built. 

See sample of this in action at http://laurenrabaino.com/verge/newsletter.

If you are looking to use this mechanism to power a new newsletter, the best option is probably to clone the entire repo and replace `html.html`, `plaintext.html` and `index.html` with HTML, CSS, JS, tabletop variables, etc of your own style and from a different Google Spreadsheet.

### Dependencies
 - jQuery: http://jquery.com/
 - tabletop.js: https://github.com/jsoma/tabletop
 - handlebars.js: http://handlebarsjs.com/

## Set up

### 1. Create the Google spreadsheet
Create a Google doc that will serve as the lightweight database to power the content of the email newsletter. Each column of the spreadsheet will represent a variable that the template loops over to populate the newsletter. 

The Verge's newsletter is set up with the following sheets and columns:

#### Sheet: editorsnote

This sheet powers the top section of the newsletter, above the first ad. If the editor's note is present, it will display. If the editor's not is empty, the markup will not display. It is also the sheet that controls the content of the lead story. 

Columns:

 - **editorsnote:** If populated with *any value*, the editor's note markup displays; if empty, editor's note markup does not display.
 - **leadhed:** Plain text for the headline of the lead story
 - **leadlink:** URL for the lead story, which will wrap around the lead image and headline.
 - **leadimageurl:** Image URL for the lead image. Grabbed from Chorus. 
 - **leadsummary:** Blurb that goes with lead story.
 - **advertisementon:** If populated with *any value*, the markup for the top advertisement will display; if empty, markup will not display. 

#### Sheet: featured stories

This sheet powers the middle section containing three big stories, one of which will occassionally be a Harmony sponsored post. 

Columns:

- **featuredhed:** Headlines for the featured stories go in this column. 
- **featuredurl:** URL for features stories go in this column. 
- **featuredimgurl:** URL for featured stories goes here. This is grabbed from Chorus. 
- **featuredblurb:** Hand-crafted blurb for featured stories goes here.
- **harmony:** If populated with *any value*, the corresponding story in that row will get a 'FROM OUR SPONSOR' label, a 'Powered by Vox Creative' byline and a right-aligned sponsored title bar. 

#### Sheet: storylist

This powers the numbered list of headlines that appears in the lowermost section of the email newsletter. 

Columns:

- **storyhed:** Headline for the first story
- **storyurl:** URL for the story
- **storyblurb:** Blurb for the story
- **number:** Sailthru image URL for the numbered burst icon

#### Sheet: advertiseon

This is a custom sheet for the bottom variable advertisement. 

Columns:

- **advertiseon:** If populated with *any* value, markup for displaying ad will render in markup. If empty, ad will not display. 

### 2. Publish spreadsheet. 

For tabletop/handlebars to access data from the spreadsheet, it needs to be publishing to the web. To make the sheet public, go to `File > Publish to the web > All sheets > Start publishing`. It will give you a URL that looks something like this: https://docs.google.com/spreadsheet/pub?key=0Audl93NY80BvdDNUdWJtWUJOb2FNVmFXSEo4eVJOVWc&output=html. 

Copy the link to the published data, which you will use in `index.html`, `html.hml` and `plaintext.html` in the next step. 

## 3. Creating your templates.

After your email newsletter markup has been written and throughly tested in multiple email browsers, you can create various versions of it that will power this tool. 

### index.html
This page is what displays the preview of the newsletter and provides HTML and plain text markup for editorial to paste into sailthru. The contents of it will actually never go into the email newsletter, but it gives editors/producers a visual way of seeing the content that they've entered into the spreadsheet in an actual marked up display that's closely representative of how the email newsletter itself will display. 

The top of this page contains two iframes: one that displays the contents of `html.html`, which is the markup that will be pasted into the code view of the sailthru template/campaign; one that displays the contents of `plaintext.html` which goes into the plaintext tab of the template/campaign. 

Notes:

- After you have marked up this page fully with dummy content, follow the [documentation for tabletop.js](https://github.com/jsoma/tabletop) to get the variables from your spreadsheet to render within the template. 
- It's usually best practice to keep the styles in &lt;style&gt; tags on this view. 



### html.html
This page displays the encoded html, which will render decoded so it can be copied/pasted into sailthru. 

You need to do three specific steps to generate the markup for this page: 

1. Run your source code from index.html (excluding the iframe embed section at the top) through MailChimp's CSS inliner tool: http://templates.mailchimp.com/resources/inline-css/. The reason for this is because, as you will quickly learn, most email clients only work if the styles are inline. Keep the style tags as well, for the media queries. 

2. After you've run your source code through the inliner, then run it through the HTML encoder at http://htmlentities.net/. This is because when the content displays in the iframe "grab this code" box, we don't want it to render the html, we want the source code to display. So you have to encode all your code. 

3. Go through and add all the correct tabletop.js variables, includes, etc. that you did for `index.html` so that upon render, the actual content will show up instead of tabletop variables. 


### plaintext.html
This page displays the plain text, stripped-down version of the email. This should just be a simple "headline: [url]" treatment for stories and share/about/optout URLs. 

## 4. Host this thang somewhere.
If you want editors/producers to have a persistent place through which to preview the email newsletter, it should be hosted somewhere. Verge temporarily hosts theirs at: http://laurenrabaino.com/verge/newsletter, but you could host it on a sanbox or S3 or wherever. 

## Resources

#### Guides
- Responsive email design best practices: http://www.campaignmonitor.com/guides/mobile/
- Email marketing field guide: http://mailchimp.com/resources/guides/email-marketing-field-guide/

#### Tools
- Mailchimp inliner tool (useful for the html.html page): http://templates.mailchimp.com/resources/inline-css/
- Converting HTML entities (useful for the html.html page): http://htmlentities.net/
- Test multiple third-party browsers (we don't currently pay for this): https://litmus.com/

#### Compatibility 
- Which email clients support media queries? https://litmus.com/help/email-clients/media-query-support/
- Which email clients support web fonts? http://www.campaignmonitor.com/resources/will-it-work/webfonts/

#### Troubleshooting
- Yahoo Mail sucks: http://www.emailonacid.com/blog/details/C13/stop_yahoo_mail_from_rendering_your_media_queries


