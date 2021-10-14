# UI Developer A11Y Knowledge Article Overview

## Abbreviations

• AT = Assistive Technology

• SR = Screen Reader

• a11y - description for anything about accessibility.

## Users

• Screen Reader Users

• Keyboard only users

• Colorblind, Low-vision users

## Tools to use for 508

### Axe Extension

• for Chrome:
https://chrome.google.com/webstore/detail/axe-devtools-web-accessib/lhdoppojpmngadmnindnejefpokejbdd?hl=en-US

• for Firefox
https://addons.mozilla.org/en-US/firefox/addon/axe-devtools/

### Color Contrast Analyzer

https://webaim.org/resources/contrastchecker/

## Workflow for fixing accessiblity issues

1. pull up the axe core run test from Test engineer. Analyze the report.

2. Navigate to the project in INT or local in either Chrome or Firefox

3. use the axe extension to audit site for accessiblity, cross-referencing the axe core report. NOTE: there most likely will be some false positives with contrast ratios.
4. Be prepared to check contrast issues with the color contrast analyzer to make sure that the site has a correct ratio.
5. make sure to fix the Critical, and Serious issues first. Then moderate.
6. The needs review issues are there because the extension can't difinitively tell if there is an issue or not, so you will have to manually check.

## What AT users will expect in regards to navigation

1. Users will use tab key for navigation of actionable elements. So buttons, links, inputs, ect.. should all be tabbable and have a visual focus border so that sighted users can see where they are on the page.
2. Users will use arrow keys to navigate to non-actionable elements.
3. Links and buttons should be actionable with the enter/spacebar.
4. radio/checkbox elements are a little different. the first one or default selected option in the group is tabbable, and the others are navigatable with the arrows. Selection of an element should be the spacebar.
5. select elements are different as well. Users will expect that the element be triggered with the spacebar, and then navigate through all the options with the arrow keys. The arrow functionality should be trapped until a selection is made.
6. Some users use headings to navigate which is why it is important that they are in order. Navigating by headings for users should be like reading an outline of the page.

## Techniques for fixing common issues

### Images

1. A decorative image - an image that does not add any context to the experience. Purely decorative. in this case an empty alt attribute is preferred. Example: background image, some icons.

2. A Contextual image - an image that adds some context or value to a sighted user. With this type of image, an alt attribute with a descriptive value is preferred. A description needs to be concise and descriptive. Not overly verbose. Example: image with words or an icon that adds context like a social media icon, or a phone or email icon. Note: a contextual image nested inside a link that has valid text, does not need a description. An empty alt attribute is preferred.

### Headings

1. there needs to be at least one h1 on a page that describes the page.

2. the best practice is to not skip headings. For example: coding an h3 right after an h1 is bad practice. There should be an h2 after an h1, and likewise down to h6. There are ways to fix this issue:

   1. add the attributes: aria-level="whatever level the heading should be" and role="heading". this will make a screen reader announce the level it should be.

   2. use the correct heading markup and then use vuetify's helper classes for text size. "text-h1, text-h2, text-h3, ect..."

### Actionable Elements

It is always better, to use an interactive element like a link or a button. These interactive elements come with both the ability to be tabbed to, and keyboard functionality. As far as which one to use? It is usually better to use links when the action is to navigate somewhere, and buttons when an action on the page is needed like opening a menu or modal.

1. Avoid using clickable divs. This can create many a11y issues:

   1. divs are not interactive elements so adding the ability to tab to the element is necessary for Keyboard and AT users.
   2. divs also are not keyboard activatable so code would have to be written to make sure that an AT/Keyboard user can interact with it.

2. Modals:

   1. Focus needs to be managed when a modal is present. When the modal button trigger is clicked, focus needs to be shifted to the close button of the modal.
   2. Tabbing also needs to be trapped inside the modal so the AT/Keyboard user cant access the content behind the modal.
   3. Focus needs to be shifted back to the original trigger when the modal is closed. For reference: <a href="<https://www.w3.org/TR/wai-aria-practices-1.1/examples/dialog-modal/dialog.html>" target="_blank">https://www.w3.org/TR/wai-aria-practices-1.1/examples/dialog-modal/dialog.html</a>
