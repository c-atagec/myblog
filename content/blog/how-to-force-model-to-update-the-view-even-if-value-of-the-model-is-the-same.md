+++
title = "How to Force Model to Update the View Even if Value of the Model Is the Same"
date = "2021-09-06T05:54:23+03:00"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["block","css","display","google-fonts","inline","inline-block","local-fonts","none","scss","vue-3","vue-3-project-setup","vue-cli-4","vue.config.js","vuejs",]
+++

In our Vue application sometimes we want to manipulate the DOM in a non-Vue way.

For example, after directly changing inline styles to create an animation, Vue's model doesn't reflect the actual content in the DOM, to get around this we can 'refresh' the model to let the Vue take control of the DOM again.

By the end of this blog post, you'll learn how to force model to update the view, even if the value of the model is the same, with **:key changing technique.**

Before we dive in, I want to mention that there are other ways to force component to re-render, such as using **v-if** or **forceUpdate** method. We are focusing specifically **:key changing technique** in this post. To read about other techniques you can check out Michael Thiessen's [article](https://michaelnthiessen.com/force-re-render/), he goes into great detail while explaining the use cases.

Now, onto the blog post.

## What is key changing technique?

First of all let's start with defining what is the `key`.

In Vue, `key` is a special attribute that allows Vue's virtual DOM algorithm to **detect changes**. It can also be used to re-render elements instead of just patching changes.

So, every time a key changes it will force element to re-render and trigger it's lifecycle hooks. In other words, if one of the elements in the DOM doesn't reflect the model, we can use key attribute to make Vue **take control** over the DOM again.

## How To Use :key Attribute

Here we have two examples of how to use key property. First method is directly mutating **data property** and using it in the template for styling of the element. 

Second method also mutates data property but instead, we are using computed property to do the styling for us. In both cases, we are using a **unique key** to force component to **re-render**.

Also, keep in mind that we are manually styling ( manipulating ) the DOM, to demonstrate how to refresh the model in the cases of Vue's model is not synced with the actual DOM content. You can use any other approach ( other than styling ) to manipulate DOM.

Now, let's take a look at each method individually.

Using `data property` to style the template.

```html
<template>
  <!-- We are using 'color' data to style <p> element -->
  <p id="color-item" :style="{ color: color }" :key="refreshKey">Color: {{ color }}</p>
  <!-- Call refreshModel method to update the key -->
  <button @click="refreshModel()">Refresh Model</button>
</template>

<script>
  export default {
    name: "App",
    data() {
      return {
        color: "red",
        refreshKey: 0
      };
    },
    mounted() {
      // Manipulate DOM content manually when component is mounted
      const item = document.getElementById("color-item");
      item.style.color = "blue";
    },
    methods: {
      refreshModel() {
        // Change key value to trigger re-render
        this.refreshKey += 1;
      }
    }
  };
</script>
```

Here, we are initializing our component with **color** and **refreshKey** properties. Which has values **red** and **0** respectively. We are using `:style` attribute apply styling and giving a `:key` with the value of **refreshKey** ( which is 0 at the initial load ).

Then, when component is mounted we are **manually** manipulating the DOM by setting **#color-item**'s color to **blue**.

When button is clicked we are calling refreshModel method, and changing it's value. This will update the `:key` attribute, and trigger component to re-render, which will set **#color-item**'s color to **red** again.



And this is how we achieve same result with **computed property**.

```html
<template>
  <!-- We are using 'computedColor' property to style <p> element -->
	<p id="color-item" :style="{ color: computedColor }" :key="refreshKey">Color: {{ computedColor }}</p>
   <!-- Call refreshModel method to update the key -->
  <button @click="refreshModel ()">Refresh Model</button>
</template>

<script>
export default {
  name: "App",
   data() {
    return {
      color: "red",
      refreshKey: 0
    };
  },
  mounted() {
		// Manipulate DOM content manually when component is mounted
    const item = document.getElementById("color-item");
    item.style.color = "blue";
  },
  methods: {
    refreshModel () {
      // Change key value to trigger re-render 
      this.refreshKey += 1;
    }
  },
  computed: {
    computedColor() {
      return this.color;
    }
  }
};
</script>
```

Let's take a look how it's going to look after we implemented the changes in our code.

![model-update](/images/vue-model-update/model-update.gif)

As we can see, when the component is mounted, **#color-item**'s color changed to **blue**. But component's model remains unchanged. Which rendered as `{{color}}` and `{{computedColor}}` in the template.

When we click **Refresh Model** button, `refreshKey` is updated. Thus triggered a re-render and let the Vue take over the DOM again.


## Conclusion and live examples

It's important to understand Vue's reactivity system and best leverage from it while avoiding manuel DOM manipulation as much as possible. Of course, there are some cases ( and will be ) where manually changing the DOM content becomes unavoidable. 

Even then, we should lean into Vue's reactivity system as much as possible to prevent unwanted scenarios. For that, you look up [reactivity section](https://v3.vuejs.org/guide/reactivity.html#reactivity-in-depth) in the docs, to refresh your memory time to time.

Also, make sure to check out the codepen examples to see code snippets in action from the previous section.

`data property example`

{{< codepen id="WNOGMoV" >}}

`computed property example`

{{< codepen id="qBjaKbX" >}}

Which both is going to look same as a result. But you can inspect the code to see subtle differences between the two approaches.