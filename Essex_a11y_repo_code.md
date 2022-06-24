#fixes #a11y

# A11y Code Examples

## a11y - Skip to Main link

1. add link element with class of skipLink and href="#main" to App.vue
2. Add id="main" to v-main component

```vue
<template>
	<v-app v-resize="() => $store.dispatch('width')">
		<div class=" skip text-left">
			<a href="#main" ref="skipLink" class="skipLink text-left">Skip to main content</a>
		</div>
		<!-- nav bar -->
		<nav-bar-base />

		<!-- side bar -->
		<side-bar-base
			v-if="($store.getters['auth/authenticated'] && $route.name !== 'login') || skipAuth"
		/>

		<!-- page view -->
		<transition name="page" mode="out-in">
			<v-main :key="$route.name" id="main">
				<router-view />
			</v-main>
		</transition>

		<!-- nav bar -->
		<footer-base />

		<!-- global components -->
		<timeout-guard v-if="!skipAuth" />
		<snackbar />
	</v-app>
</template>
```

3. add styles for link

```scss
<style lang="scss">
/* skip link */

.skipLink {
	white-space: nowrap;
	margin: 1em auto;
	top: 0;
	left: 4%;
	position: fixed;
	opacity: 0;
}
.skipLink:focus {
	opacity: 1;
	background-color: white;
	padding: 0.5em;
	border: 3px solid color(aqua-lighten1);
	z-index: 10;
}
</style>
```



## a11y - caption for data tables

DataTable.vue -

```js
export default {
  // add caption prop
  props: {
    caption: String,
  }
  mounted() {
  /*
    1. create caption element with offscreen text class
    2. set captionText to the caption prop
    3. prepend caption element to the table as first child
  */
    if (this.caption) {
      console.log('there is a caption')
      let captionElement = document.createElement('caption')
      captionElement.setAttribute('class', 'd-sr-only')
      let captionText = document.createTextNode(this.caption)
      captionElement.appendChild(captionText)
      let wrapper = this.$refs.table.$el.querySelector('table')
      if (captionText !== '') {
        wrapper.prepend(captionElement)
      }
    }
  }
}
```

file where table-wrapper exists

```vue
<!-- set caption prop on instance of table wrapper -->
<data-table-wrapper
  v-bind="CNVTable"
  class="empty-dash"
  caption="Copy Number Variants (CNV) Un-normalized"
/>
```





## Using DialogMixin for a11y functionality

This mixin applies a tab loop for dialogs, so that SR users can't exit the dialog with a tab key press. It also adds an aria-label to the dialog element for SR user announcement 

1. create the DialogMixin.js file

```js
export default {
	props: {
		value: {
			type: Boolean,
			default: false,
		},
		attach: {
			type: String,
			required: true,
		},
	},

	methods: {
		$_refocus(el) {
			// wait for preview dialog to close
			setTimeout(() => {
				if (el) {
					el.focus()
				} else {
					this.$refs.$_refocus.$el.focus()
				}
			}, 1)
		},
		cleanup() {
			let dialog = document.querySelector(this.attach + ' > [role="dialog"]')
			if (!dialog) return
			// remove tabstops
			if (dialog.previousElementSibling) dialog.previousElementSibling.remove()
			if (dialog.nextElementSibling) dialog.nextElementSibling.remove()
		},
	},

	watch: {
		value: {
			immediate: true,
			handler(value) {
				if (value) {
					// wait for dialog animation
					setTimeout(() => {
						let dialog = document.querySelector(this.attach + ' > [role="dialog"]')
						dialog.setAttribute(
							'aria-label',
							this.$refs.dialogLabel.innerText || this.$refs.dialogLabel.textContent?.trim()
						)
						dialog.removeAttribute('tabindex')
						dialog.insertAdjacentHTML('beforebegin', '<div class="tabstop" tabindex="0"></div>')
						dialog.insertAdjacentHTML('afterend', '<div class="tabstop" tabindex="0"></div>')
						let focusables = dialog.querySelectorAll(
							'button:not([disabled]), input, select, textarea, div[tabindex="0"]:not(.tabstop)'
						)
						focusables[0].focus()
						let lastFocusable = focusables[focusables.length - 1]
						// tabstop #1
						dialog.previousElementSibling.addEventListener('focus', () => {
							lastFocusable.focus()
						})
					}, 1)
				} else {
					// parents using only v-model will inherit a falsy value when dialog closes
					this.cleanup()
				}
			},
		},
	},

	// parents using v-if will destroy the dialog
	beforeDestroy() {
		this.cleanup()
	},
}

```

2. create a div with an id to attach the dialog to.

3. Add the dialog component with the attach prop. Set the value of the prop to the div id

   

   ```vue
   	<div id="com_doc_attach"></div>
   		<CommentsDocumentsDialog
   			v-model="CommsDocsDialogOpen"
   			:participantId="participantId"
   			attach="#com_doc_attach"
   		/>
   ```

   

4. in the dialog component add the attach prop to the dialog in the template
5. set a ref prop with the value of dialogLabel to the header of the dialog
6. add aria-label that describes what the close button does.

```vue
<v-dialog
		v-model="value"
		id="commDocDialog"
		:content-class="`commDocDialog overflow-dialog selected-comments--${selectedComments}`"
		class="commDocDialog pb-5"
		width="1200"
		@click:outside="$emit('input', false)"
		:attach="attach"
	>
		<v-card class="dialog-card commDocDialog pb-15">
			<v-layout class="dialog-card__header pt-3 mb-3 mt-0" align-center>
				<div class="text-h5 my-3" ref="dialogLabel">Comments & Documents</div>
				<v-spacer />
				<v-btn
					@click="$emit('input', false), (openAddCommentDocument = false)"
					icon
					aria-label="Close Dialog"
				>
					<v-icon>fa-times</v-icon>
				</v-btn>
      </v-layout>
		</v-card>
</v-dialog>
```

## AutocompleteMixin a11y fix

This mixin handles adding offscreen text that describes to a SR user how to navigate the autocomplete menu. We are lucky that vuetify handles the keyboard interaction here. all we really need to do is describe the way to navigate.

```js
// a11y remediation for autocomplete component
export default {
	data() {
		return {
			// search: '',
			a11yAC: false,
		}
	},

	methods: {
		autocomplete_a11y() {
			const listbox = document.querySelector('.v-autocomplete__content')
			const ariaLive = document.createElement('div')
			ariaLive.className = 'd-sr-only'
			ariaLive.innerHTML = 'to navigate autocomplete options, use up and down arrow keys.'
			ariaLive.setAttribute('aria-live', 'polite')
			ariaLive.setAttribute('role', 'status')
			listbox.append(ariaLive)
		},
	},
	watch: {
		search: {
			handler(search) {
				if (search !== '' && this.a11yAC == false) {
					this.a11yAC = true
					setTimeout(() => {
						this.autocomplete_a11y()
					}, 2000)
				}
			},
			deep: true,
		},
	},
}
```

