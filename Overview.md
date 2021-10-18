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

### Avoid using non-semantic HTML code

• when there is a list use ul, ol, dl elements

• use buttons and links instead of clickable divs

• use semantic form controls

• Vuetify elements are good to, as most of the time they have some built in accessibility functionality.

### Axe Extension

• for Chrome:
https://chrome.google.com/webstore/detail/axe-devtools-web-accessib/lhdoppojpmngadmnindnejefpokejbdd?hl=en-US

• for Firefox
https://addons.mozilla.org/en-US/firefox/addon/axe-devtools/

### Color Contrast Analyzer

https://webaim.org/resources/contrastchecker/

### Axe Linter

this tool will help to identify problems in the codebase. Looking for accessibility issues.

https://marketplace.visualstudio.com/items?itemName=deque-systems.vscode-axe-linter

Note: The linter is currently flagging an accessibility error on the rule: click-events-have-key-events on actionable elements with the @click in vue.js files. There is already an issue reported for this false positive, and they are looking into it. if you add an axe-linter.yml file in the root of codebase to silence this error until they can fix it. the axe-linter.yml file should have this content:

axe-linter.yml

```yml
rules:
  click-events-have-key-events: false
```

## Workflow for fixing accessiblity issues

1. pull up the axe core run test from Test engineer. Analyze the report.

2. Navigate to the project in INT or local in either Chrome or Firefox

3. use the axe extension to audit site for accessiblity, cross-referencing the axe core report. NOTE: there most likely will be some false positives with contrast ratios.
4. Be prepared to check contrast issues with the color contrast analyzer to make sure that the site has a correct ratio.
5. make sure to fix the Critical, and Serious issues first. Then moderate.
6. The needs review issues are there because the extension can't difinitively tell if there is an issue or not, so you will have to manually check these issues. They may or may not be an issue.

## AT/Keyboard User Expectations

1. Users will use tab key for navigation of actionable elements. So buttons, links, inputs, ect.. should all be tabbable and have a visual focus border so that sighted users can see where they are on the page.
2. Users will use arrow keys to navigate to non-actionable elements.
3. Links and buttons should be actionable with the enter/spacebar.
4. radio/checkbox elements are a little different. the first one or default selected option in the group is tabbable, and the others are navigatable with the arrows. Selection of an element should be the spacebar.
5. select elements are different as well. Users will expect that the element be triggered with the spacebar, and then navigate through all the options with the arrow keys. The arrow functionality should be trapped until a selection is made.
6. Some users use headings to navigate which is why it is important that they are in order. Navigating by headings creates an 'outline' of the page which is why it is important for the headings to be in order.
