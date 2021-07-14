# UI Developer A11Y Knowledge Article

Abbreviations

• AT = Assistive Technology

• SR = Screen Reader

• a11y - description for anything about accessibility

## 1.1 What is WAI-ARIA?

WAI-ARIA, the Accessible Rich Internet Applications Suite, defines a way to make Web content and Web applications more accessible to people with disabilities. It especially helps with dynamic content and advanced user interface controls developed with HTML, JavaScript, and related technologies.

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

• ARIA uses can cover up or override the original semantics or content.

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

## 2.1 Use Cases

• Checkboxes/ Radio Buttons

• Headings

• Toggle Buttons

• Accordions

• Tables

• Dialogs

• Links

• Landmarks

• aria-labelledby

• aria-describedby

• aria-label

• hiding semantics

• keyboard best practices

## 3.1 Using Axe Linter/Axe Core/ Axe Extension

## Axe Linter

## Axe Core

## Axe Extension
