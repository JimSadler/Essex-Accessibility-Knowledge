# Testing and Developing with Accessibility in Mind

## Abbreviations

- AT = Assistive Technology
- SR = Screen Reader
- a11y - description for anything about accessibility.

## Users

The World Health Organization estimates that 15% of the world's population has some range of disability, and up to 4% of those are significantly disabled. That is about 1 billion people worldwide!

There is a wide range of disabilities that can be catagorize as

- Visual - These users benefit from the use of screen readers, screen magnification, screen contrast, and refreshable braille displays.
- Auditory - These users benefit from captioning, transcripts or sign language videos
- Motor - These users benefit from a range of assistive technologies for motor impairments: adaptive keyboards, oversized trackball mouse, sip and puff switch, head wand, single-switch access, eye tracking, voice recognition software, etc...
- Cognitive - these users benefit from supplemental media, structural organization of content, clear and simple writing.

**_more reading:_**

- <a href="https://www.w3.org/WAI/perspective-videos/" target="_blank">Perspective Videos</a>
- <a href="https://www.w3.org/WAI/people-use-web/user-stories/"  target="_blank">User Stories</a>

<br>

---

<br>

## Manual testing needs to accompany automated testing

Deque's automated testing solution (axe core/axe extension) catches about 57% of digital accessibility issues, which is great, but automated testing will not be able to flag keyboard-only navigation, Descriptive Content, or User Experience.

1. **_Keyboard-only navigation_** - Navigating a website without a mouse is necessary for many users that have disabilities. Being able to access page menus, interact with links/buttons/pop-up windows(dialogs), ect.. using only keyboard commands, is essential for Test Engineers and Developers to have the knowledge to be able to test the website with the keyboard / screenreaders. To achieve this, test engineers and front end developers need to understand how to use a screen reader, and how a keyboard only user navigates a webpage.
2. **_Descriptive Content_** - Titles, headings and alt text all must be appropriately descriptive. Scans can tell you weather these attributes/elements are present, but they will not be able to assess if these elements are appropriate for the page.
3. **_User Experience_** - Automated testing can not interact with a website the way that a user would. For example: Consider testing the website for submitting an assay/variant ect.. an automated testing tool can not interact with the website and click the submit button, or select the correct assay to submit. A manual tester can do this and simulate the process finding any a11y issues present.
4. **_False Negatives_** - automated tests will inevitably report an issue that is not an issue, the axe extension tries to aleviate this by catagorizing the issues that it can not difinitively say are an issue into a 'Needs Review' category. But these 'Needs Reviews' need to be manually tested by test engineers/developers to make sure that there are no a11y issues.

<br>

---

<br>

## Where to focus your energy

There just is not enough time to conduct extensive manual accessibility testing on every page of the site, because the site is constantly changing and may or may not have many pages. The most efficient way to manual test is to focus on the following types of pages:

- Site-wide Templates
- Representative Pages that include

  - Informational Images
  - Active Images
  - Forms
  - Data Tables
  - Multimedia
  - Dynamic Pages
  - Modal Windows
  - Color
  - Timers
  - Different Languages
  - Critical Pages
    - Key Entry Points
    - Key User Paths
    - Highest Traffic Pages
    - The Obvious
    - Feedback Forms
    - Accessibility Policy Pages

  <br>
  this comes from Glenda Sims at Deque University. To read more about this:
  <a href="https://www.deque.com/blog/manual-accessibility-testing-approach/" target="_blank">Accessibility: Manual Testing Approach</a>

---

<br>

## AT/Keyboard User Expectations

- Users will use tab key for navigation of actionable elements. So buttons, links, inputs, ect.. should all be tabbable and have a visual focus border so that sighted users can see where they are on the page.
- Users will use arrow keys to navigate to non-actionable elements.
- Links and buttons should be actionable with the enter/spacebar.
- radio/checkbox elements are a little different. the first one or default selected option in the group is tabbable, and the others are navigatable with the arrows. Selection of an element should be the spacebar.
- select elements are different as well. Users will expect that the element be triggered with the spacebar, and then navigate through all the options with the arrow keys. The arrow functionality should be trapped until a selection is made.
- Some users use headings to navigate which is why it is important that they are in order. Navigating by headings creates an 'outline' of the page.

alot of this is handled by Vuetify, however they should be manually tested to make sure.

<br>

---

<br>

## Developers - Workflow for fixing accessiblity issues using the Axe Extension( after a 508 testing session )

1. pull up the axe core run test from Test engineer. Analyze the report.
2. Navigate to the project in INT or local in either Chrome or Firefox
3. use the axe extension to audit site for accessiblity, cross-referencing the axe core report. NOTE: there most likely will be some false positives with contrast ratios.
4. Be prepared to check contrast issues with the color contrast analyzer to make sure that the site has a correct ratio.
5. make sure to fix the Critical, and Serious issues first. Then moderate.
6. The needs review issues are there because the extension can't definitively tell if there is an issue or not, so you will have to manually check these issues. They may or may not be an issue.
7. there are some a11y issues that are not found by the extension, or by the 508 test suite for example focus management on dialogs. Dialogs should be developed with a11y in mind. A good resource for this: https://www.w3.org/TR/wai-aria-practices-1.1/examples/dialog-modal/dialog.html

<br>

---

<br>

## Skip Link

A skip link at the top of each page that goes directly to the main content area is helpful for users so they can skip content that is repeated on multiple pages for example, Page navigation.

note: this is not required, as long as there is another way that the user can navigate, for example a compliant heading structure ( see content structure section )

EXAMPLE:

Typically this is done on the top of the App.vue as it will be the first focusable element on all your pages:

```html
<ul class="skip-links">
  <li>
    <a href="#main" ref="skipLink">Skip to main content</a>
  </li>
</ul>
```

to hide the link unless it is focused:

```css
.skipLink {
  white-space: nowrap;
  margin: 1em auto;
  top: 0;
  position: fixed;
  left: 50%;
  margin-left: -72px;
  opacity: 0;
}
.skipLink:focus {
  opacity: 1;
  background-color: white;
  padding: 0.5em;
  border: 1px solid black;
}
```

and shifting the focus back to the skip link when the user changes the route:

```html
<script>
  export default {
    watch: {
      $route() {
        this.$refs.skipLink.focus()
      }
    }
  }
</script>
```

<br>

---

<br>

## Content Structure

Content structure is very important for accessiblity.

### Headings

Users can navigate an application through headings. Having descriptive headings for every section makes it easier for users to predict the content of each section. When it comes to headings there are some recommended accessibility practices:

1. Nest headings in their ranking order.

2. there needs to be at least one h1 on a page that describes the page.

3. the best practice is to not skip headings. For example: coding an h3 right after an h1 is bad practice. There should be an h2 after an h1, and likewise down to h6. There are ways to fix this issue:

   1. add the attributes: aria-level="_whatever level the heading should be_" and role="heading". this will make a screen reader announce the level it should be.

   2. use the correct heading markup and then use vuetify's helper classes for text size. "text-h1, text-h2, text-h3, ect..."

4. Use actual heading tags instead of styling text to give the visual appearance of headings

```html
<main role="main" aria-labelledby="main-title">
  <h1 id="main-title">Main title</h1>
  <section aria-labelledby="section-title">
    <h2 id="section-title">Section Title</h2>
    <h3>Section Subtitle</h3>
    <!-- Content -->
  </section>
  <section aria-labelledby="section-title">
    <h2 id="section-title">Section Title</h2>
    <h3>Section Subtitle</h3>
    <!-- Content -->
    <h3>Section Subtitle</h3>
    <!-- Content -->
  </section>
</main>
```

Using ARIA we can fix heading order issues using this technique:

```html
<div class="container">
  <h1>Heading 1</h1>
  <h3 role="heading" aria-level="2">Heading 3</h3>
  <!--  heading level 2 -->
  <h5 role="heading" aria-level="3">Heading 5</h5>
  <!--  heading level 3 -->
  <h2 role="heading" aria-level="4">Heading 2</h2>
  <!--  heading level 4 -->
  <h6 role="heading" aria-level="5">Heading 6</h6>
  <!--  heading level 5 -->
  <h1 role="heading" aria-level="6">Heading 1</h1>
  <!--  heading level 6 -->
</div>

<!-- this technique does not affect the styles of the headings.
 It just dictates the heading level for the screen reader. -->
```

<div class="container">

### Landmarks

Landmarks provide programmatic access to sections within an application. Users that rely on Assistive Technology (AT) can use landmarks to navigate each section of the website and skip over content.

Several HTML sectioning elements automatically create ARIA landmark regions. So, in order to provide assistive technology users with a logical view of a page, it is important to understand the effects of using HTML sectioning elements. To be clear using semantic HTML elements will create landmarks automatically. In the event that you are not using semantic HTML sectioning elements, ARIA landmark roles can to be used.

#### **_Landmark Design_**

- Assign landmark roles based on the type of content in area
- banner, main, complimentary and contentinfo landmarks should be top level landmarks
- if a landmark is used multiple times on a page, like multiple navigation landmarks, each instance should have a unique label.

**_note:_** do not use the name of the landmark in the label as that will create redundancy with the SR announcement.

| HTML    | ARIA Role            | Landmark Purpose                                                                                                 |
| ------- | -------------------- | ---------------------------------------------------------------------------------------------------------------- |
| header  | role="banner"        | Prime Heading: title of the page                                                                                 |
| nav     | role="navigation     | Collection of links suitable for use when navigating the document or related documents                           |
| main    | role="main"          | The main or central content of the document                                                                      |
| footer  | role="contentinfo"   | Information about the parent document footnotes/copyrights/links to privacy statement                            |
| aside   | role="complimentary" | Supports the main content, yet is separated and meaningful on its own content                                    |
| N/A     | role="search"        | This section contains the search functionality for the application                                               |
| form    | role="form"          | Collection of form-associated elements                                                                           |
| section | role="region"        | Content that is relevant and that users will likely want to navigate to. Label must be provided for this element |

**_Banner_**

- header ---> role="banner" - when in context of the body element
- The banner landmark identifies site oriented content at the top of the page. Most often this is referred to as the header.
- Things that may be included in the banner: Logo, Main navigation, search.
- the banner landmark should be a top level landmark
- There can only be one banner landmark on the page, with the exception of a nested iframe which can also have a banner landmark. In the event of multiple banner landmarks, a unique label needs to be applied so that AT users can differentiate between the two.

```html

<!-- ••• HTML ••* -->
<body>
    <header>
      <h1>page title identifying website<h2>
      .... header area content ....
    </header>
</body>

<!-- ••* ARIA ••* -->

<div role="banner">
  <h2>page title identifying website<h2>
  .... header area content ....
</div>
```

**_Navigation_**

Navigation landmarks provide a way to identify groups (e.g. lists) of links that are intended to be used for website or page content navigation.

- nav ---> role="navigation"
- If a page contains more than one navigation landmark, then a unique label should be used
- If a navigation landmark has an identical set of links as another navigation landmark, then use the same label for each landmark

```html
<!-- ••• HTML ••* -->
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

<!-- ••• ARIA ••* -->
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

**_Main_**

A main landmark identifies the primary content of the page.

- main ---> role="main"
- Each page should have one main landmark
- The main landmark should be a top level landmark
- When a page contains nested document and/or application roles (e.g. typically through the use of iframe and frame elements), each document or application role may have one main landmark.
- If a page has multiple main landmarks, each landmark should have a unique label

```html

<!-- ••• HTML ••* -->
<main>
  <h1>title for main content<h1>
  .... main content area ....
</main>

••* HTML Multiple landmarks

<main aria-labelledby="title1">
  <h1 id="title1">title for main content area 1<h1>
  .... main content area 1 ....
</main>

....

<main aria-labelledby="title2">
  <h1 id="title2">title for main content area 2<h1>
  .... main content area 2 ....
</main>


<!-- ••* ARIA ••* -->
<div role="main">
  <h1>title for main content<h1>
  .... main content area ....
</div>

••* ARIA Multiple landmarks
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

**_Contentinfo_**

A contentinfo landmark is a way to identify common information at the bottom of each page within a website, typically called the "footer" of the page, including information such as copyrights and links to privacy and accessibility statements.

- footer ---> role="contentinfo" - when in context of the body element
- The contentinfo landmark should be a top level landmark
- Each page should only have one contentinfo landmark
- There can be multiple contentinfo landmarks when an embedded iframe/frame elements, and in that case each landmark should have unique labels

```html
<!-- ••• HTML ••* -->
<body>
    <footer>
      <h2>Contact, Policies and Legal<h2>
      .... contentinfo area content ....
    </footer>
</body>

<!-- ••* ARIA ••* -->
<div role="contentinfo">
  <h2>Contact, Policies and Legal<h2>
  .... contentinfo area content ....
</div>
```

**_Complementary_**

- aside ---> role="complimentary"
- A complimentary landmark is meant as a supporting section of the document, the main content
- complementary landmarks should be top level landmarks
- if there are more than one complementary landmarks, then a unique label should be used.

```html
<!-- ••* HTML ••* -->
  <aside >
    <h2>Title for complementary area<h2>
    .... complementary area content ....
  </aside>

<!-- ••* ARIA ••* -->

<div role="complementary">
  <h2>Title for complementary area<h2>
  .... complementary area content ....
</div>
```

**_Search_**

The following example shows a search form with a "search" landmark. The search role typically goes on the form field or a div surrounding the search form.

```html
<form role="search">
  <label for="s6">search</label><input id="s6" type="text" size="20" />
  .... form content ....
</form>
```

**_Form_**

A form landmark identifies a region that contains a collection of items and objects that, as a whole, combine to create a form when no other named landmark is appropriate (e.g. main or search).

- form ---> role="form"
- Use the search landmark instead of the form landmark if the form is used for search functionality
- The form landmark should have a label to give context to users about form purpose
- The form label should be visible to all users
- Make sure to have a unique label for multiple form landmarks
- When ever possible, make sure that controls contained in a form landmark are semantic HTML elements.

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

**_Section_**

A region landmark is a perceivable section of the page containing content that is sufficiently important for users to be able to navigate to the section.

- section ---> role="region"
- A region landmark must have a label.
- If a page includes more than one region landmark, each should have a unique label.
- The region landmark can be used identify content that named landmarks do not appropriately describe.

```html

<!-- ••• HTML ••* -->
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

<!-- ••• ARIA ••* -->
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

<br>

---

<br>

## Accessible forms using semantic html

```html
<form action="/dataCollectionLocation" method="post" autocomplete="on">
  <div v-for="item in formItems" :key="item.id" class="form-item">
    <label :for="item.id">{{ item.label }}: </label>
    <input
      :type="item.type"
      :id="item.id"
      :name="item.id"
      v-model="item.value"
    />
  </div>
  <button type="submit">Submit</button>
</form>
```

Labels are typically placed on top of form fields.
Notice how you can include autocomplete="on" on the form element and it will apply to all inputs in the form. It is also possible to add input specific values for the autocomplete attribute :
https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/autocomplete

### Labels

Provide labels to describe the purpose of all form control; programmatically link the "for" attribute of the label to the "id" attribute of the input.

```html
<label for="name">Name</label>
<input type="text" name="name" id="name" v-model="name" />
```

<br>

#### ARIA-LABEL

You can also use an aria-label to give the input an accessible name:

```html
<label for="name">Name</label>
<input
  type="text"
  name="name"
  id="name"
  v-model="name"
  :aria-label="Full Name"
/>
<!-- this approach negates the label tag. A screen reader will only announce the aria-label;
 in this example the screenreader will announce : "Full Name"-->
```

<br>

#### ARIA-LABELLEDBY

Using aria-labelledby is similar to aria-label, except of the implementation. It is mostly used if the text that labels the input is visible on screen, and there is no programmatic connection to the input. It is implemented by using the id of the visible label text as the value of the aria-labelledby attribute. It is possible to have multiple values, which will result with the screenreader announcing the values of the attribute in order.

```html
<form
  class="demo"
  action="/dataCollectionLocation"
  method="post"
  autocomplete="on"
>
  <h1 id="billing">Billing</h1>
  <div class="form-item">
    <label for="name">Name:</label>
    <input
      type="text"
      name="name"
      id="name"
      v-model="name"
      aria-labelledby="billing name"
    />
  </div>
  <button type="submit">Submit</button>
</form>

<!-- in this example, the screenreader will announce "Billing Name:" -->
```

<br>

#### ARIA-DESCRIBEDBY

aria-describedby is used in the same way as aria-labelledby except it provides a description with additional information that the user might need.

```html
<form
  class="demo"
  action="/dataCollectionLocation"
  method="post"
  autocomplete="on"
>
  <h1 id="billing">Billing</h1>
  <div class="form-item">
    <label for="name">Full Name:</label>
    <input
      type="text"
      name="name"
      id="name"
      v-model="name"
      aria-labelledby="billing name"
      aria-describedby="nameDescription"
    />
    <p id="nameDescription">Please provide first and last name.</p>
  </div>
  <button type="submit">Submit</button>
</form>
<!-- In this example the screen reader will announce: "Billing Full Name: Please provide first and last name" -->
```

#### Instructions

When adding instructions for the input controls in your form, make sure to link it correctly to the input. You can provide additional instructions and connect multiple ids inside an aria-labelledby or aria-describedby attribute. This enables more flexability.

```html
<fieldset>
  <legend>Using aria-labelledby</legend>
  <label id="date-label" for="date">Current Date:</label>
  <input
    type="date"
    name="date"
    id="date"
    aria-labelledby="date-label date-instructions"
  />
  <p id="date-instructions">MM/DD/YYYY</p>
</fieldset>
<!-- in this example a screen reader will announce: "Current Date: MM/DD/YYYY" -->
```

```html
<fieldset>
  <legend>Using aria-describedby</legend>
  <label id="dob" for="dob">Date of Birth:</label>
  <input type="date" name="dob" id="dob" aria-describedby="dob-instructions" />
  <p id="dob-instructions">MM/DD/YYYY</p>
</fieldset>

<!-- in this example a screen reader will announce: "Current Date: MM/DD/YYYY" -->
```

#### Try to avoid:

**_Inputs nested inside labels_**

this works, but it is better supported by Assistive Technology to explicitly match the labels and inputs as the above examples

```html
<label>
  Name:
  <input type="text" name="name" id="name" v-model="name" />
</label>
```

<br>

**_Placeholders_** can confuse users. One of the issues with placeholders is that they are not announced by screenreaders, and most often do not meet the color contrast requirements. If you fix the color contrast of a placeholder the result is most often confused for pre-populated data. It is best to provide all the information that the user needs outside of the input.

#### Form Errors

Form Errors need to be addressed so that AT/Keyboard users will know what the error is. Here are a few ways to implement accessble form errors.

1. when the form is submitted and there are errors, show a box with all the errors and shift focus to the error box.
2. use an alert.
3. when errors occur on submit, show error messages above the input with the error, programatically connect the input with the error message using aria-describedby attribute, and shift focus to the first input with an error.
4. add aria-required="true" to the required inputs. (this will alert the AT user that they need to fill out this input before submitting)

Ex.

```html
<span id="err1" class="error_message">The username is not valid</span>
<label for="name">Username</label>
<input id="name" aria-required="true" aria-describedby="err1" />

<!-- this input will announce as required on first pass, and when it is submitted the user will hear " input username,  the username is not valid" once the input is refocused -->
```

### Hiding Content

**_Visually Hiding Content_**

Sometimes there is screenreader specific content that does not need to be visible.
for example in a search form that has a submit button. The submit button conveys context so there is no need to show the input label.

```html
<form role="search">
  <label for="search" class="hidden-visually">Search: </label>
  <input type="text" name="search" id="search" v-model="search" />
  <button type="submit">Search</button>
</form>
```

We can do this because the search button will help the visual users understand the purpose of the input field. We can then use CSS to visually hide elements but keep them available for assistive technologies.

```css
.hidden-visually {
  position: absolute;
  overflow: hidden;
  white-space: nowrap;
  margin: 0;
  padding: 0;
  height: 1px;
  width: 1px;
  clip: rect(0 0 0 0);
  clip-path: inset(100%);
}
```

The above is also a good technique to add screen reader only text for more context to a link or button.

```html
<a href="#"
  >Read More
  <span class="hidden-visually"> about this particular subject</span>
</a>

<!-- this technique will visually show the text "Read More", and the screen reader will announce
 "Read More about this particular subject" -->
```

**_Hiding Content from Screen reader_**

Adding aria-hidden="true" will hide the element from assistive technology(AT) but leave it visually available.

**_Only use this on decorative, duplicated, or offscreen content._**.

```html
<form role="search">
  <label for="searchIcon" class="hidden-visually">Search: </label>
  <input type="text" name="searchIcon" id="searchIcon" v-model="searchIcon" />
  <button type="submit">
    <i class="fas fa-search" aria-hidden="true"></i>
    <span class="hidden-visually">Search</span>
  </button>
</form>
<!-- notice the use of aria-hidden on the search icon.
 Also note the visually-hidden text in the button -->
```

<br>

---

<br>

## Images

1. A decorative image - an image that does not add any context to the experience. Purely decorative. in this case an empty alt attribute is preferred. Example: background image, some icons that do not offer any context.

```html
<img alt="" src="decorative_image.jpg" />
```

2. A contextual image - an image that adds some context or value to a sighted user. With this type of image, an alt attribute with a descriptive value is preferred. A description needs to be concise and descriptive. Not overly verbose. Example: image with words or an icon that adds context like a social media icon, or a phone or email icon. Note: a contextual image nested inside a link that has valid text, does not need a description. An empty alt attribute is preferred.

```html
<img alt="This image creates context" src="contextual_image.jpg" />
```

<br>

---

<br>

## Actionable Elements

It is always better, to use an interactive element like a link or a button. These interactive elements come with both the ability to be tabbed to, and keyboard functionality. As far as which one to use? It is usually better to use links when the action is to navigate somewhere, and buttons when an action on the page is needed like opening a menu or modal.

Avoid using clickable divs. This can create many a11y issues

1. divs are not interactive elements so adding the ability to tab to the element is necessary for Keyboard and AT users.
2. divs also are not keyboard activatable so code would have to be written to make sure that an AT/Keyboard user can interact with it.
3. Using a semantic actionable element handles both of the concerns with a clickable div. this makes it an easy fix.

### Modals

1. Focus needs to be managed when a modal is present. When the modal button trigger is clicked, focus needs to be shifted to the close button of the modal. A robust best practice here would be to add aria-describedby to the button with the id of the Header of the modal so that the user would hear the title of the modal. (not necessary, but a nice touch)
2. Tabbing also needs to be trapped inside the modal so the AT/Keyboard user cant access the content behind the modal. A couple of considerations here are to take into account when the user is tabbing forward through the modal or shift+tabbing backward through the modal.
3. the close button needs to have screenreader text. A quick solution would be to add an aria-label="close"
4. Focus needs to be shifted back to the original trigger when the modal is closed.

For reference: <a href="<https://www.w3.org/TR/wai-aria-practices-1.1/examples/dialog-modal/dialog.html>" target="_blank">https://www.w3.org/TR/wai-aria-practices-1.1/examples/dialog-modal/dialog.html</a>

<br>

---

Vuetify does handle many a11y solutions for keyboard users built in their components.
<a href="https://vuetifyjs.com/en/features/accessibility/#additional-resources"  target="_blank">For more information on how Vuetify implements Accessiblity</a>

<br>

## Resources

### Documentation

- Web Content Accessibility Guidelines (WCAG)

  - <a href="https://www.w3.org/TR/WCAG20/"  target="_blank">WCAG 2.0</a>

- Web Accessibility Initiative - Accessible Rich Internet Applications(WAI-ARIA)
  - <a href="https://www.w3.org/TR/wai-aria-1.2/"  target="_blank">Accessible Rich Internet Applications (ARIA)</a>
  - <a href="https://www.w3.org/TR/wai-aria-practices-1.2/"  target="_blank">ARIA Authoring Practices</a>
  - <a href="https://material.io/design/usability/accessibility.html#hierarchy"  target="_blank">Material Design Accessiblity</a>

### Tools to use for 508

- Screenreaders
  - <a href="https://www.apple.com/accessibility/mac/vision/">VoiceOver</a>
  - <a href="https://www.nvaccess.org/download/">NVDA</a>
  - <a href="https://www.freedomscientific.com/products/software/jaws/?utm_term=jaws%20screen%20reader&utm_source=adwords&utm_campaign=All+Products&utm_medium=ppc&hsa_tgt=kwd-394361346638&hsa_cam=200218713&hsa_ad=296201131673&hsa_kw=jaws%20screen%20reader&hsa_grp=52663682111&hsa_net=adwords&hsa_mt=e&hsa_src=g&hsa_acc=1684996396&hsa_ver=3&gclid=Cj0KCQjwnv71BRCOARIsAIkxW9HXKQ6kKNQD0q8a_1TXSJXnIuUyb65KJeTWmtS6BH96-5he9dsNq6oaAh6UEALw_wcB">JAWS</a>

* Extensions (for testing browser pages for a11y)

  - <a href="https://chrome.google.com/webstore/detail/axe-devtools-web-accessib/lhdoppojpmngadmnindnejefpokejbdd?hl=en-US" target="_blank"> Axe Extension for Chrome</a>
  - <a href="https://addons.mozilla.org/en-US/firefox/addon/axe-devtools/" target="_blank">Axe Extension for Firefox</a>
  - <a href="https://wave.webaim.org/extension/" target="_blank">WebAim WAVE Extension</a>

* Color Contrast Tools
  - <a href="https://webaim.org/resources/contrastchecker/"  target="_blank">WebAim Color Contrast </a>
  - <a href="https://webaim.org/resources/linkcontrastchecker"  target="_blank">WebAim Link Color Contrast</a>

- Automated Tools

  - <a href="https://marketplace.visualstudio.com/items?itemName=deque-systems.vscode-axe-linter"  target="_blank">Axe Linter (developers)</a> this tool will help to identify problems in the codebase. Looking for accessibility issues. Note: The linter is currently flagging an accessibility error on the rule: click-events-have-key-events on actionable elements with the @click in vue.js files. There is already an issue reported for this false positive, and they are looking into it. if you add an axe-linter.yml file in the root of codebase to silence this error until they can fix it. the axe-linter.yml file should have this content:

    axe-linter.yml

    ```yml
    rules:
      click-events-have-key-events: false
    ```

* Other Helpful Extention Tools
  - <a href="https://chrome.google.com/webstore/detail/nerdefocus/lpfiljldhgjecfepfljnbjnbjfhennpd?hl=en-US…"  target="_blank">NerdeFocus</a>
  - <a href="https://chrome.google.com/webstore/detail/headingsmap/flbjommegcjonpdmenkdiocclhjacmbi?hl=en…"  target="_blank">Heading Map</a>
