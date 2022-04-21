#fixes #a11y

# A11y Code Examples

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



## Fix for Vuetify Accordions bug

Background: Vuetify tries to make their expansion panels accessible. They make one mistake on initial load of the component -> they add aria-expanded to the wrapper that contains the button, and not on the button itself. This creates a situation where the user will not know that the content is expandable. They will just know that there is a button, but not that the button is expandable, until after the button is clicked. This fix removes the aria-expanded from the wrapper on initial load of the page, and adds it to the button element which fixes the issue

1. Create AriaUtil.js file if not already created.
2. Add new panelChange function and export for use in entire project

```js
export function panelChange(panel) {
  setTimeout(() => {
    // remove aria-label from wrapper
    let wrapper = panel?.$el
    wrapper.removeAttribute('aria-expanded')

    // set aria-label on button
    let button = panel?.$el?.querySelector('button')
    if (button.getAttribute('aria-expanded') === null) {
      button.setAttribute('aria-expanded', false)
      wrapper.removeAttribute('aria-expanded')
    }
  }, 10)
}
```

3. import into file that contains Vuetify expanded panels

   ```js
   import { panelChange } from '@/utils/AriaUtil'
   ```

4. add ref attribute to v-expansion-panel, and add a change event with the value of the ref as the parameter in the panelChange function

```vue
<v-expansion-panels flat>
		<v-expansion-panel ref="assignedPanel" @change="panelChange($refs.assignedPanel)">
			<v-expansion-panel-header v-slot:default="{ open }" class="" aria-expanded="false"
				><div>
					<div class="strata_blue d-inline">
						<span v-if="!open" key="0" class="ml-1 pt-3 pr-15"
							>{{ regimenTotal }} treatment regimens</span
						>
						<span v-else key="1" class="ml-5 py-0">hide treatment regimens</span>
					</div>
				</div>
			</v-expansion-panel-header>
			<v-expansion-panel-content>
				<div
					class="d-flex justify-start regimens-heading"
					v-for="(item, i) in regimenData"
					:key="i"
				>
					<div class="regimens-blue py-3 pl-1">
						<v-icon class="tab-icon px-3" small>fas fa-pills</v-icon>
						<span class="subhead ml-1 text-left">{{ item.r_num }} </span
						><span class="ml-5">{{ item.name }}</span>
					</div>

					<div class="regimen-icon text-center py-0 px-2">
						<v-icon class="tab-icon" style="margin-top: -5px !important;" small>fas fa-user</v-icon>
						<div class="number">{{ item.num }}</div>
					</div>
				</div>
			</v-expansion-panel-content>
		</v-expansion-panel>
	</v-expansion-panels>
```

4. Add panelChange with reference to template to mounted section. Also add panelChange as a method

   ```js
   export default {
     mounted() {
       panelChange(this.$refs.assignedPanel)
     },
     methods: {
       panelChange
     }
   }
   ```
