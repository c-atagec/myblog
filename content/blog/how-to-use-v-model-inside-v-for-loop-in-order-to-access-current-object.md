+++
title = "How to Use v-model Inside v-for Loop in Order to Access Current Object"
date = "2021-09-22T06:38:25+03:00"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["block","css","display","google-fonts","inline","inline-block","local-fonts","none","scss","vue-3","vue-3-project-setup","vue-cli-4","vue.config.js","vuejs",]
+++


One of the Vue's capabilities is two-way data binding. Which means when the value in the DOM changes ( e.g. an input value ) , **data changes,** when the data changes **DOM content changes.** That way we don't have to worry about checking if our data is synced with the DOM.

For example, if we have a input element in the template, we can bind it to a data model like this:

```jsx
data() {
	return {
    lyrics: ''
  }
}
```

```jsx
<input v-model="lyrics" placeholder="sing a song" />
<p>Lyrics are: {{ lyrics}}</p>
```

That, at first glance seems pretty straightforward. But most of the time, we have a bunch of data that needs to be used with form bindings.

By the end of this blog post, you'll learn what **v-model** is and also how to use it inside v-for in order to access current object.

Before we dive into the use cases, let's have a look at what v-model is.

## What is v-model?

In essence, **v-model** is a directive, that allows us to create two-way data binding with form elements ( e.g. input, textarea, select ). What that means is, when the data is changed by the user ( via the input ), or by the code, data always stays in sync. This is called **two-way data binding**.

Here's how it looks on a live example:

![v-model-data-binding](/images/v-model-v-for/v-model-input.gif)


You can check this [sandbox](https://codesandbox.io/s/vue-3-v-model-input-gq2t6?file=/src/App.vue) to examine the source code. 

## Example of uses with v-model

### Scenario 1: Changing data model directly with v-model

In this scenario, we're going to take a look at how to use v-model to change our existing data model.

`data model`
```jsx
data() {
	return {
        // Our data model
		doors: [
        {
          id: 1,
          name: "door 1",
          open: true
        },
        {
          id: 2,
          name: "door 2",
          open: false
        },
        {
          id: 3,
          name: "door 3",
          open: false
        },
        {
          id: 4,
          name: "door 4",
          open: true
        }
      ]
	}
}
```

`template`

```html
<ul class="doors">
    <li class="door" v-for="door in doors" :key="door.id">
      <input class="door--input" type="checkbox" :name="door.name" :value="door.name" :id="door.id" :checked="door.open" v-model="door.open"/>
    </li>
  </ul>
```

Let's explain what's happening here:

- We are displaying every item with **v-for** directive.
- Accessing current object model to bind data to the checkboxes.
- Then with **v-model** directive we are changing the current item's **door.open** parameter.

Here's how the end result looks like.

![v-model-checkboxes](/images/v-model-v-for/v-model-checkboxes.gif)


As we can see by using v-model directive we were able to change the data model according to the user actions.

You can also check the source code from [here](https://codesandbox.io/s/input-v-model-example-omhok?file=/src/App.vue).

### Scenario 2: Saving user selections to another array/object

In this use case, we'll have a look at how to **save** user selections to another **array/object**. This might be useful for if you just want to send selections ( not the whole data model ), to the server.

### Array Syntax

`data model`
```jsx
data() {
    return {
      configuration: [
        {
          name: "Choose exterior color",
          options: [
            { option: "white" },
            { option: "silver" },
            { option: "red" },
            { option: "orange" },
          ],
        },
        {
          name: "Choose wheel size",
          options: [
            { option: "21''" },
            { option: "20''" },
            { option: "19''" },
            { option: "18''" },
          ],
        },
        {
          name: "Choose interior color",
          options: [
            { option: "brown" },
            { option: "black" },
            { option: "bordeaux" },
            { option: "beige" },
          ],
        },
        {
          name: "Choose seat type",
          options: [
            { option: "power spot seats" },
            { option: "sport seats plus" },
            { option: "adaptive sport seats" },
            { option: "full bucket seats" },
          ],
        },
      ],
      selectedOptions: [],
    };
  },
```

`template`
```html
<ul class="configuration">
    <li
      class="config"
      v-for="(config, index) in configuration"
      :key="config.name"
    >
      <p class="config-name">{{ config.name }}</p>
      <ul class="option-list">
        <li
          class="option-item"
          v-for="item in config.options"
          :key="item.option"
        >
          <input
            type="radio"
            :name="config.name"
            :id="item.option"
            :value="item.option"
            v-model="selectedOptions[index]"
          />
          <label for="item.option">{{ item.option }}</label>
        </li>
      </ul>
    </li>
  </ul>
  <p class="selected-options">
    <strong>Selected options</strong><br />
    {{ selectedOptions }}
  </p>
```

Here's how it looks on a live example:

![v-model-array-syntax](/images/v-model-v-for/v-model-array-syntax.gif)


By accessing parent index we are saving each option into it's corresponding position in the array.

```jsx
// index parameter comes from parent element
v-model="selectedOptions[index]"
```

### Object Syntax

We can also save user selections into an object. What we can do to achieve this is, just by changing these lines.

 
`data model`
```jsx
data() {
	return {
		...
		selectedOptions: []
  }
}
```

`template`
```html
<input type="radio" :name="config.name" :id="item.option" :value="item.option"
       v-model="selectedOptions[index]"
    />
```

To these:

`data model`
```jsx
data() {
	return {
		...
		selectedOptions: {}
  }
}
```

`template`
```html
<input type="radio" :name="config.name" :id="item.option" :value="item.option"
    v-model="selectedOptions['option' + index]"
  />
```

In our data model, we **initialized** selectedOptions as an **object,** and in our template we accessed the parent index to save our selections into corresponding property. Which will look like this as a result:

![v-model-object-syntax](/images/v-model-v-for/v-model-object-syntax.gif)


### Conclusion and Demo Codes
Understanding two-way data binding is essential for us deepen our knowledge about Vue's reactivity. It can also easify the process when we are working with forms.

To practice **v-model** and you can examine the source code of examples:
- [v-model input data binding](https://codesandbox.io/s/vue-3-v-model-input-gq2t6?file=/src/App.vue)
- [Mutating existing data model](https://codesandbox.io/s/input-v-model-example-omhok?file=/src/App.vue)
- [Saving selections - array syntax](https://codesandbox.io/s/input-v-model-save-data-array-w1e51?file=/src/App.vue)
- [Saving selections - object syntax](https://codesandbox.io/s/input-v-model-save-data-t8k6c?file=/src/App.vue)


