# Techniques for fixing common issues

Vuetify handles the majority of accessiblility concerns. YAY! Good for Vuetify. That being said there are a few concerns, and alternative fixes that are beneficial.

## Images

1. A decorative image - an image that does not add any context to the experience. Purely decorative. in this case an empty alt attribute is preferred. Example: background image, some icons.

2. A contextual image - an image that adds some context or value to a sighted user. With this type of image, an alt attribute with a descriptive value is preferred. A description needs to be concise and descriptive. Not overly verbose. Example: image with words or an icon that adds context like a social media icon, or a phone or email icon. Note: a contextual image nested inside a link that has valid text, does not need a description. An empty alt attribute is preferred.

## Headings

1. there needs to be at least one h1 on a page that describes the page.

2. the best practice is to not skip headings. For example: coding an h3 right after an h1 is bad practice. There should be an h2 after an h1, and likewise down to h6. There are ways to fix this issue:

   1. add the attributes: aria-level="_whatever level the heading should be_" and role="heading". this will make a screen reader announce the level it should be.

   2. use the correct heading markup and then use vuetify's helper classes for text size. "text-h1, text-h2, text-h3, ect..."

## Actionable Elements

It is always better, to use an interactive element like a link or a button. These interactive elements come with both the ability to be tabbed to, and keyboard functionality. As far as which one to use? It is usually better to use links when the action is to navigate somewhere, and buttons when an action on the page is needed like opening a menu or modal.

### Avoid using clickable divs. This can create many a11y issues

1. divs are not interactive elements so adding the ability to tab to the element is necessary for Keyboard and AT users.
2. divs also are not keyboard activatable so code would have to be written to make sure that an AT/Keyboard user can interact with it.
3. Using a semantic actionable element handles both of the concerns with a clickable div. this makes it an easy fix.

### Modals

1. Focus needs to be managed when a modal is present. When the modal button trigger is clicked, focus needs to be shifted to the close button of the modal. A robust best practice here would be to add aria-describedby to the button with the id of the Header of the modal so that the user would hear the title of the modal. (not necessary, but a nice touch)
2. Tabbing also needs to be trapped inside the modal so the AT/Keyboard user cant access the content behind the modal. A couple of considerations here are to take into account when the user is tabbing forward through the modal or shift+tabbing backward through the modal.
3. the close button needs to have screenreader text. A quick solution would be to add an aria-label="close"
4. Focus needs to be shifted back to the original trigger when the modal is closed.

For reference: <a href="<https://www.w3.org/TR/wai-aria-practices-1.1/examples/dialog-modal/dialog.html>" target="_blank">https://www.w3.org/TR/wai-aria-practices-1.1/examples/dialog-modal/dialog.html</a>

### Form Elements

• All form control elements need to be programatically connected to the labels associated with them - use the label for attribute to connect the input id attribute.
• Vuetify does a great job with input labels, as long as the developer uses the label property during implementation.

Ex:

```html
<label for="email">Email</label> <input id="email" type="text" />
```

Vuetify form elments are great for accessiblity because the input label is persistant meaning even when the user is editing the input the label is present and visible

#### Form Errors

Form Errors depending on how they are developed need to be addressed so that AT/Keyboard users will know what the error is. A couple of solutions:

1. when the form is submitted and there are errors, show a box with all the errors and shift focus to the error box.
2. use an alert. Vuetify has a great solution for errors.
3. when errors occur on submit, show error messages above the input with the error, programatically connect the input with the error message using aria-describedby attribute, and shift focus to the first input with an error.

Ex.

```html
<span id="err1" class="error_message">The username is not valid</span>
<label for="name">Username</label>
<input id="name" aria-describedby="err1" />
```

#### Radio groups

checkbox/radio groups need to have a description available so that the user knows what they are selecting.

• Using a fieldset element with a legend element is the conventional way to do this.
• Vuetify's radio-group component handles this well by using a div with role="radiogroup" and then a legend with the addition of v-slot: label

Ex.

```html
<template>
  <v-container fluid>
    <v-radio-group v-model="radios">
      <template v-slot:label>
        <div>Your favourite <strong>search engine</strong></div>
      </template>
      <v-radio value="Google">
        <template v-slot:label>
          <div>
            Of course it's <strong class="success--text">Google</strong>
          </div>
        </template>
      </v-radio>
      <v-radio value="Duckduckgo">
        <template v-slot:label>
          <div>
            Definitely <strong class="primary--text">Duckduckgo</strong>
          </div>
        </template>
      </v-radio>
    </v-radio-group>
  </v-container>
</template>
```
