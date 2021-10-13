# UI Developer A11Y Knowledge Article

Abbreviations

• AT = Assistive Technology

• SR = Screen Reader

• a11y - description for anything about accessibility

## 1.1 What is WAI-ARIA?

WAI-ARIA, the **_A_**ccessible **_R_**ich **_I_**nternet **_A_**pplications Suite, defines a way to make Web content and Web applications more accessible to people with disabilities. It especially helps with dynamic content and advanced user interface controls developed with HTML, JavaScript, and related technologies.

## 1.2 ARIA provides UI developers with

• 1.2.1 Roles to describe the type of widget presented (menu, slider, progressbar, etc.… )

• 1.2.2 Roles to describe the structure of a web page (heading, regions, tables, ect….)

• 1.2.3 Properties to describe the state widgets are in (checked, pressed, haspopup, etc….)

• 1.2.4 Properties to define live regions of a web page that are likely to get continuous updates like stock quotes, etc….

• 1.2.5 A way to provide keyboard navigation for the Web objects and events, such as those mentioned above.

## 1.3 ARIA does not provide

• 1.3.1 Keyboard functionality

• 1.3.2 Focus

• 1.3.3 Color Contrast

## 1.4 ARIA Authoring Practices

### 1.4.1 NO ARIA is better than BAD ARIA

• semantic HTML is always better than ARIA

Ponder this code

```html
<div role="button">Place Order</div>
```

the issue here is that a div does not have any keyboard functionality, so a screen reader user can not tab to this element, and there is no javascript inherently on this element. so to make this work, javascript would have to be written so that a keyboard/screen reader user can trigger the button with the enter/space key, and adding tabindex='0' so that the keyboard user/ AT user can tab to this element

A better solution here is -

```html
<button>Place Order</button>
```

Using the HTML button tag brings with it tabability, and javascript for a keyboard click inherently

### 1.4.2 ARIA can both cloak and enhance, creating both Power and Danger

• ARIA implementation can cover up or override the original semantics or content.

```html
<a href="#" role="menuitem"
  >This element will be interpreted by AT as a item in a menu, not a link
</a>

<a
  href="#"
  aria-label="AT users will only hear the contents of this aria-label. They will not here the text of the link"
  >Link Text</a
>
```

• On the other hand, some uses of ARIA add meaning to elements that are essential to AT users.

```html
<button aria-pressed="false">Mute</button>
```

• Both of the above examples showcase the power of what ARIA can do - enables developers to describe nearly any component in a way that AT can accurately interpret, making the components accessible.

• The above examples also show the danger of using ARIA incorrectly

## 2 Use Cases

### 2.1 ARIA Authoring Practices

<a href="https://www.w3.org/TR/wai-aria-practices-1.1/#aria_ex" target="_blank"> Specific use cases for ARIA</a>

### 2.2 Specific Use Cases

• <a href="https://www.w3.org/TR/wai-aria-practices-1.1/#names_and_descriptions" target="_blank">Providing Accessible Names and Descriptions</a>

• <a href="https://www.w3.org/TR/wai-aria-practices-1.1/#aria_ex" target="_blank">Design Patterns</a>

• <a href="https://www.w3.org/TR/WCAG20-TECHS/aria#ARIA7" target="_blank">aria-labelledby</a>

• <a href="https://www.w3.org/TR/WCAG20-TECHS/aria#ARIA1" target="_blank"> aria-describedby</a>

• <a href="https://www.w3.org/TR/wai-aria-practices-1.1/#naming_with_aria-label" target="_blank">aria-label</a>

• <a href="https://www.w3.org/TR/wai-aria-practices-1.1/#keyboard" target="_blank">keyboard best practices</a>

• <a href="https://www.w3.org/TR/wai-aria-practices-1.1/#accordion" target="_blank">Accordions (Expanding Panels)</a>

**_NOTE_**

When using expansion panel component, Vuetify takes care of this automatically for the UI in that the user knows that there is expanded content. The implementation that Vuetify uses, in my opinion is flawed. The reason that this approach is flawed:

• Notice in the example below, that Vuetify adds aria-expanded="false" to the div container contains the button, and then adds aria-expanded="true" to the button when the button is clicked.

• The SR user will not know that there is expandable content untill after the button is clicked.

**Example:**

```html
<div aria-expanded="false" class="v-expansion-panel">
  <button
    type="button"
    class="v-expansion-panel-header v-expansion-panel-header--mousedown"
  >
    Item
    <div class="v-expansion-panel-header__icon">
      <span aria-hidden="true" class="v-icon notranslate theme--light">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 24 24"
          role="img"
          aria-hidden="true"
          class="v-icon__svg"
        >
          <path
            d="M7.41,8.58L12,13.17L16.59,8.58L18,10L12,16L6,10L7.41,8.58Z"
          ></path>
        </svg>
      </span>
    </div>
  </button>
</div>
```

**_Solution_**

• A better solution would be to put aria-expanded="false" on the button instead of the container, and then manage the state of the attribute when the button is clicked. That way the user will hear - " Button, collapsed" when the button gets focus, and then "Button, expanded" when the button is clicked.

```html
<div class="v-expansion-panel">
  <button
    type="button"
    class="v-expansion-panel-header v-expansion-panel-header--mousedown"
    aria-expanded="false"
  >
    Item
    <div class="v-expansion-panel-header__icon">
      <span aria-hidden="true" class="v-icon notranslate theme--light">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 24 24"
          role="img"
          aria-hidden="true"
          class="v-icon__svg"
        >
          <path
            d="M7.41,8.58L12,13.17L16.59,8.58L18,10L12,16L6,10L7.41,8.58Z"
          ></path>
        </svg>
      </span>
    </div>
  </button>
</div>
```

• <a href="https://www.w3.org/TR/WCAG20-TECHS/aria#ARIA12" target="_blank">Headings</a>

Heading levels are a way that AT users can navigate a webpage. It creates an outline that they can use for context. The heading levels need to be in order for this functionality. There are a few guidelines here to keep in mind:

• Skipping heading levels is a bad practice: h1 -> h2 -> h3 -> h4 -> h5 -> h6
• using role="heading" in conjunction with aria-level is a valid way to fix heading order issues.

**_Example_**

• notice that the html heading tags below are out of order. This would be a 508 violation.

```html
<div class="container">
  <h1>Heading 1</h1>
  <h3>Heading 2</h3>
  <h5>Heading 3</h5>
  <h2>Heading 4</h2>
  <h6>Heading 5</h6>
  <h1>Heading 6</h1>
</div>
```

**_Solution_**

to fix this: we need to add both role="heading" and aria-level="correct heading level"
if role="heading" is not applied, then some SR will not interpret the element correctly

```html
<div class="container">
  <h1>Heading 1</h1>
  <h3 role="heading" aria-level="2">Heading 2</h3>
  <h5 role="heading" aria-level="3">Heading 3</h5>
  <h2 role="heading" aria-level="4">Heading 4</h2>
  <h6 role="heading" aria-level="5">Heading 5</h6>
  <h1 role="heading" aria-level="6">Heading 6</h1>
</div>
```

• <a href="https://www.w3.org/TR/WCAG20-TECHS/aria#ARIA11" target="_blank">Landmarks</a>

Several HTML sectioning elements automatically create ARIA landmark regions. So, in order to provide assistive technology users with a logical view of a page, it is important to understand the effects of using HTML sectioning elements. To be clear using semantic HTML elements will create landmarks automatically. In the event that you are not using semantic HTML sectioning elements, ARIA landmarks need to be used.

Semantic HTML sectioning elements offer automatic landmarks when used correctly.

**_Complementary_**

• aside ---> role="complimentary"

• A complimentary landmark is meant as a supporting section of the document, the main content

• complementary landmarks should be top level landmarks

• if there are more than one complementary landmarks, then a unique label should be used.

```html
••• HTML •••
  <aside >
    <h2>Title for complementary area<h2>
    .... complementary area content ....
  </aside>

••• ARIA •••

<div role="complementary">
  <h2>Title for complementary area<h2>
  .... complementary area content ....
</div>
```

**_Contentinfo_**

A contentinfo landmark is a way to identify common information at the bottom of each page within a website, typically called the "footer" of the page, including information such as copyrights and links to privacy and accessibility statements.

• footer ---> role="contentinfo" - when in context of the body element

• The contentinfo landmark should be a top level landmark

• Each page should only have one contentinfo landmark

• There can be multiple contentinfo landmarks when an embedded iframe/frame elements, and in that case each landmark should have unique labels

```html
••• HTML •••
<body>
    <footer>
      <h2>Contact, Policies and Legal<h2>
      .... contentinfo area content ....
    </footer>
</body>

••• ARIA •••
<div role="contentinfo">
  <h2>Contact, Policies and Legal<h2>
  .... contentinfo area content ....
</div>
```

**_Banner_**

• header ---> role="banner" - when in context of the body element

• The Banner landmark identifies site oriented content at the top of the page. Most often this is referred to as the header.

• Things that may be included in the banner: Logo, Main navigation, search.

• the banner landmark should be a top level landmark

• There can only be one banner landmark on the page, with the exception of a nested iframe which can also have a banner landmark. In the event of multiple banner landmarks, a unique label needs to be applied so that AT users can differentiate between the two.

```html

••• HTML •••
<body>
    <header>
      <h1>page title identifying website<h2>
      .... contentinfo area content ....
    </header>
</body>

••• ARIA •••

<div role="banner">
  <h2>page title identifying website<h2>
  .... contentinfo area content ....
</div>
```

**_Main_**

A main landmark identifies the primary content of the page.

• main ---> role="main"

• Each page should have one main landmark

• The main landmark should be a top level landmark

• When a page contains nested document and/or application roles (e.g. typically through the use of iframe and frame elements), each document or application role may have one main landmark.

• If a page has multiple main landmarks, each landmark should have a unique label

```html

••• HTML •••
<main>
  <h1>title for main content<h1>
  .... main content area ....
</main>

••• HTML Multiple landmarks

<main aria-labelledby="title1">
  <h1 id="title1">title for main content area 1<h1>
  .... main content area 1 ....
</main>

....

<main aria-labelledby="title2">
  <h1 id="title2">title for main content area 2<h1>
  .... main content area 2 ....
</main>


••• ARIA •••
<div role="main">
  <h1>title for main content<h1>
  .... main content area ....
</div>

••• ARIA Multiple landmarks
<div role="main" aria-labelledby="title1">
  <h1 id="title1">title for main content area 1<h1>
  .... main content area 1 ....
</div>

....
<div role="main" aria-labelledby="title2">
  <h1 id="title2">title for main content area 2<h1>
  .... main content area 2 ....
</div>

```

**_Form_**
A form landmark identifies a region that contains a collection of items and objects that, as a whole, combine to create a form when no other named landmark is appropriate (e.g. main or search).

• form ---> role="form"

• Use the search landmark instead of the form landmark if the form is used for search functionality

• The form landmark should have a label to give context to users about form purpose

• The form label should be visible to all users

• Make sure to have a unique label for multiple form landmarks

• When ever possible, make sure that controls contained in a form landmark are semantic HTML elements.

```html
<form aria-labelledby="contact">
  <fieldset>
    <legend id="contact">Add Contact</legend>
    ... form controls add contact ...
  </fieldset>
</form>

... OR ...

<div role="form" aria-labelledby="contact">
  <fieldset>
    <legend id="contact">Add Contact</legend>
    ... form controls add contact ...
  </fieldset>
</div>
```

**_Navigation_**

Navigation landmarks provide a way to identify groups (e.g. lists) of links that are intended to be used for website or page content navigation.

• nav ---> role="navigation"

• If a page contains more than one navigation landmark, then a unique label should be used

• If a navigation landmark has an identical set of links as another navigation landmark, then use the same label for each landmark

```html
••• HTML •••
<nav>
  <h2>title for navigation area<h2>
  <ul>
    <li><a href="page1.html">Link 1</a></li>
    <li><a href="page2.html">Link 2</a></li>
    <li><a href="page3.html">Link 3</a></li>
    <li><a href="page4.html">Link 4</a></li>
    .....
  </ul>
</nav>

••• ARIA •••
<div role="navigation">
  <h2>title for navigation area<h2>
  <ul>
    <li><a href="page1.html">Link 1</a></li>
    <li><a href="page2.html">Link 2</a></li>
    <li><a href="page3.html">Link 3</a></li>
    <li><a href="page4.html">Link 4</a></li>
    .....
  </ul>
</div>
```

**_Section_**

A region landmark is a perceivable section of the page containing content that is sufficiently important for users to be able to navigate to the section.

• section ---> role="region"

• A region landmark must have a label.

• If a page includes more than one region landmark, each should have a unique label.

•The region landmark can be used identify content that named landmarks do not appropriately describe.

```html

••• HTML •••
<main>
  <h1>title for main content area<h1>

  <section aria-labelledby="region1">
    <h2 id="region1">title for region area 1</h2>
    ... content for region area 1 ...
  </section>

  <section aria-labelledby="region2">
    <h2 id="region2">title for region area 2</h2>
    ... content for region area 2 ...
  </section>

</main>

••• ARIA •••
<div role="main">

  <h1>title for main content area<h1>

  <div role="region" aria-labelledby="region1">
    <h2 id="region1">title for region area 1</h2>
    ... content for region area 1 ...
  </div>

  <div role="region" aria-labelledby="region2">
    <h2 id="region2">title for region area 2</h2>
    ... content for region area 2 ...
  </div>

</div>
```

• <a href="https://www.w3.org/TR/WCAG20-TECHS/aria#ARIA5" target="_blank">Toggle Button /Slider</a>

• <a href="https://www.w3.org/TR/wai-aria-practices-1.1/#checkbox" target="_blank">Checkboxes</a>

• <a href="https://www.w3.org/TR/wai-aria-practices-1.1/#gridAndTableProperties" target="_blank">Tables</a>

• <a href="https://www.w3.org/TR/wai-aria-practices-1.1/#dialog_modal" target="_blank">Dialogs</a>

• <a href="https://www.w3.org/TR/wai-aria-practices-1.1/#radiobutton" target="_blank">Radio Group</a>

• <a href="https://www.w3.org/TR/wai-aria-practices-1.1/#presentation_role" target="_blank">Hiding Semantics with presentation role</a>

#### 2.3.2 Landmark Design

• Assign landmark roles based on the type of content in area

• banner, main, complimentary and contentinfo landmarks should be top level landmarks

• if a landmark is used multiple times on a page, like multiple navigation landmarks, each instance should have a unique label.

**_note:_** do not use the name of the landmark in the label as that will create redundancy with the SR announcement.

#### 2.3.3.8 Search

•

•

•

•

**_Techniques_**

**_HTML_**

**_ARIA_**

**_Example_**

```html

```

• Checkboxes/ Radio Buttons

• Toggle Buttons

• Tables

• Dialogs

• Links

• aria-labelledby

• aria-describedby

• aria-label

• hiding semantics

• keyboard best practices

## 3.1 Using Axe Linter/Axe Core/ Axe Extension

## Axe Linter

## Axe Core

## Axe Extension
