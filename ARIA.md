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

• 1.3.1 Keyboard functionality

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

• <a href="https://www.w3.org/TR/WCAG20-TECHS/aria#ARIA11" target="_blank">Landmarks</a>

Several HTML sectioning elements automatically create ARIA landmark regions. So, in order to provide assistive technology users with a logical view of a page, it is important to understand the effects of using HTML sectioning elements. To be clear using semantic HTML elements will create landmarks automatically. In the event that you are not using semantic HTML sectioning elements, ARIA landmarks need to be used.

Semantic HTML sectioning elements offer automatic landmarks when used correctly.

• <a href="https://www.w3.org/TR/WCAG20-TECHS/aria#ARIA5" target="_blank">Toggle Button /Slider</a>

• <a href="https://www.w3.org/TR/wai-aria-practices-1.1/#checkbox" target="_blank">Checkboxes</a>

• <a href="https://www.w3.org/TR/wai-aria-practices-1.1/#gridAndTableProperties" target="_blank">Tables</a>

• <a href="https://www.w3.org/TR/wai-aria-practices-1.1/#dialog_modal" target="_blank">Dialogs</a>

• <a href="https://www.w3.org/TR/wai-aria-practices-1.1/#radiobutton" target="_blank">Radio Group</a>

• <a href="https://www.w3.org/TR/wai-aria-practices-1.1/#presentation_role" target="_blank">Hiding Semantics with presentation role</a>

**_Techniques_**

**_HTML_**

**_ARIA_**

**_Example_**

```html

```

• Checkboxes/ Radio Buttons

• Toggle Buttons

• Tables

## 3.1 Using Axe Linter/Axe Core/ Axe Extension

## Axe Linter

## Axe Core

## Axe Extension
