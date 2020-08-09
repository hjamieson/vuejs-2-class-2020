# Vue JS 2 Notes
Classwork for Vue.js 2 Academy: Learn Vue Step by Step  
by Chris Dixon, Packt Publishing


## Installing vue.js
1. [Include vue in your html:](http://vuejs.org)
   ```html
   <!-- development version, includes helpful console warnings -->
   <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
   
   <!-- production version, optimized for size and speed -->
   <script src="https://cdn.jsdelivr.net/npm/vue"></script>
   ```
1. add a new Vue() object in your app.js and bind it to the html element.
  ```javascript
  new Vue({
     el: '#app',
     data: {
       event: {
         eventDate: 'August 14th - 16th',
         eventTitle: 'Summer Festival!',
         eventDescription: "It's back! This years summer festival will be in the    beautiful countryside featuring our best line up ever!"
       }
     }
   });
   ```

## Two-way Data Binding with v-model
v-model directoives provide 2-way data binding.
1. for the field element, set the v-model:  
   `...placeholder="Jane Doe" v-model="newNameText">`
1. add the model to the data object in the vue object:
   ```javascript
   new Vue({
     el: '#app',
     data: {
       event: {
         eventDate: 'August 14th - 16th',
         eventTitle: 'Summer Festival!',
         eventDescription: "It's back! This years summer festival will be in the    beautiful countryside featuring our best line up ever!"
       },
       newNameText: ''
     }
   });
   ```

## Form Submit
1. add the v-on:submit.prevent to the form:
    ```html
    <form class="form-inline" v-on:submit.prevent="formSubmitted">
      <input type="text" id="nameInput" class="form-control mb-2 mr-sm-2 mb-sm-0" placeholder="Jane Doe" v-model="newNameText">
      <input type="submit" class="btn btn-primary"></input>
    </form>
    ```
1. add a `methods` object to the Vue object with your function:
   ```js
   new Vue({
     el: '#app',
     data: {
        ...,
       newNameText: '',
       guestName: []
     },
     methods: {
       formSubmitted: function() {
         this.guestName.push(this.newNameText);
         this.newNameText = '';
       }
     }
   });
   ```

## List Rendering
You can use the `v-for` directive to iterate thru items in an array.
   * place the __v-for__ directive on the outer element that contains the array.
   * select a name to use to reference each array element.
   * use regular interpolation to render the elements of the array
   ```html
   <div class="card-success rounded name-box" v-for="name in guestName">
      {{ name }}
   </div>
   ```

## Conditional Rendering
You may want to render contitionally, such as not rendering an empty array.  Use the `v-if` directive for this.
   * place the __v-if__ directive on a parent element where the condition is tested.
   * set the condition.
   * place the __v-else__ on the alternative element to be displayed.
   * note - the placement of the __v-else__ is critical; it must be on a child element of the same level as the __v-if__.
   ```js
   <div class="card-block">
     <div v-if="guestName.length > 0">
       <div class="card-success rounded name-box" v-for="name in ">
         {{ name }}
       </div>
     </div>
     <div v-else>
       <h4>No names added yet...</h4>
     </div>
   </div>
   ```

## Binding Attributes to Elements
You cannot ad dynamic attribute to html using the {{ value }} syntax.  You can use the `v-bind` directive to apply dynamic values to html attributes such as class.
* remove the {{ value }} (if you are following the example).
* create a value in the `Vue.data` object to bind with.
* add the v-bind:class="" to the element.
```js
<div class="card-header" v-bind:class="formSubmitClass">
```
* edit your js:
```js
  methods: {
    formSubmitted: function () {
      if (this.newNameText.length > 0) {
        this.guestName.push(this.newNameText);
        this.newNameText = '';
        this.formSubmitClass = 'submitted';
      }
```
* You can use v-bind:style to apply dymanic styling to elements.
* use the same techique as class.
   ```js
   <div class="row" v-bind:style="appStyles">
      /* in the js */
       guestName: [],
       formSubmitClass: "",
       appStyles: {
         marginTop: '25px'
       }
   
   ```
* note that you must use camelcase form to get the correct css key names!
  
## Vue Shorthand Syntax
You can use the :directive shorthand with Vue.  
* `v-bind:style` becomes `:style`
* `v-bind:class` become `:class`
* `v-on:submit` becomes `@submit`

## Outputting Text & HTML
* use `v-text` to update element text
  ```js
  <div v-text="underEventDate"></div>
  /* don't forget to add the data elements! */
  ```
* use `v-html` to update elements innerHtml
  ```js
   <p class="card-text" v-html="event.eventExclusiveText"></p>
   /* don't forget to define event.eventExclusiveTest in js */
   ```
* v-once `to only` render data once.  for example, you may not want to make requests to remote if you expect the same static response:
   ```js
   <p class="card-text" v-once>{{somethingFromRemoteServer}}</p>
   ```

## v-if vs v-show
* `v-if` does _not_ render anything if the condition is false.
* `v-show` will render regardless, but will set the css attribute __display: none__ to make the element invisible

## Using JavaScript Expressions
you can place js expressions in the {{ }} braces to generate text in elements.  These expressions cannot be complex if-then-else expressions.
   ```js
   <div class="card-success rounded name-box" v-for="name in guestName">
      {{ name }}
      {{ 35 + 2 }}
      {{ Math.random() }}
   </div>

   <p class="card-text" v-html="Math.random()"></p>
   ```
   