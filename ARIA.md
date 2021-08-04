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

## 2 Use Cases with Vuetify

### 2.1 • Accordions

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

### • 2.2 Headings

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

### • 2.3 Landmarks

ARIA landmark roles provide a powerful way to identify the organization and structure of a web page. By classifying and labelling sections of a page, they enable structural information that is conveyed visually through layout to be represented programmatically. Screen readers exploit landmark roles to provide keyboard navigation to important sections of a page. Landmark regions can also be used as targets for "skip links" and by browser extensions to enhanced keyboard navigation.

This section explains how HTML sectioning elements and ARIA landmark roles are used to make it easy for assistive technology users to understand the meaning of the layout of a page.

#### 2.3.1 HTML Sectioning Elements

Several HTML sectioning elements automatically create ARIA landmark regions. So, in order to provide assistive technology users with a logical view of a page, it is important to understand the effects of using HTML sectioning elements. To be clear using semantic HTML elements will create landmarks automatically. In the event that you are not using semantic HTML sectioning elements, ARIA landmarks need to be used.

#### HTML Element Default Landmark Role

• aside ---> role="complimentary"

• footer ---> role="contentinfo" - when in context of the body element

• header ---> role="banner" - when in context of the body element

• main ---> role="main"

• nav ---> role="navigation"

• section ---> role="region"

#### 2.3.2 Landmark Design

• Assign landmark roles based on the type of content in area

• banner, main, complimentary and contentinfo landmarks should be top level landmarks

• if a landmark is used multiple times on a page, like multiple navigation landmarks, each instance should have a unique label.

**_note:_** do not use the name of the landmark in the label as that will create redundancy with the SR announcement.

#### 2.3.3 Landmark Roles

##### 2.3.3.1 Banner

• The Banner landmark identifies site oriented content at the top of the page. Most often this is referred to as the header.

• Things that may be included in the banner: Logo, Main navigation, search.

• the banner landmark should be a top level landmark

• There can only be one banner landmark on the page, with the exception of a nested iframe which can also have a banner landmark. In the event of multiple banner landmarks, a unique label needs to be applied so that AT users can differentiate between the two.

**_Techniques:_**
The HTML header element automatically defines a banner landmark when it is in context of the body. When the header element is a descendant of article, aside, section, main, or nav it is not considered a banner landmark

##### 2.3.3.2 Complementary

• A complimentary landmark is meant as a supporting section of the document, the main content

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
