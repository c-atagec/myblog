+++
title = "How to Use Jquery in Vue Component with NPM"
date = "2021-05-29T21:46:14+03:00"
description = "In this post we are going to learn to use JQuery in Vue 3 component"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["block","css","display","google-fonts","inline","inline-block","local-fonts","none","vue-3","vue-cli-4","vuejs",]
+++

In this post we are going to learn to use **JQuery** in **Vue 3 component**. For project setup you can use this [post](https://catagec.com/blog/vue-3-project-setup-with-vue-cli-4/) to get up and running.

After your project setup is completed, folder structure is going to look like this.

![vue-jquery-project-structure](/images/vue-jquery-import/vue-jquery-project-structure.png)

For this post we're just going to stick to the **App.vue component**. But you can emulate this scenario in whichever component you prefer.


## Installing JQuery using NPM
For the installation, run npm command below in your terminal.

{{< highlight sh >}}
npm install jquery
{{< /highlight >}}

This command is going to install **JQuery** into our node_modules folder.


## Importing JQuery into Vue Component
This one is pretty straightforward. Let's import our library.

{{< highlight html >}}
<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import $ from 'jquery'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

{{< /highlight >}}

We are using `$` as a placeholder to JQuery library.

We can change it and use it like this.

{{< highlight html >}}

<script>
import HelloWorld from './components/HelloWorld.vue'
import jquery from 'jquery'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

{{< /highlight >}}

Or, we can even give it a silly name like that.
{{< highlight html >}}

<script>
import HelloWorld from './components/HelloWorld.vue'
import banana from 'jquery'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

{{< /highlight >}}

Although I don't recommend it that there'll be a bunch of bananas in your code :)

## Using JQuery to Style DOM Elements
Now that we have imported it, we can access it inside our [mounted](https://v3.vuejs.org/api/options-lifecycle-hooks.html#mounted) hook.

Let's style our `<h1>`, `<h3>` and `<a>` tags to see if it's working.


{{< highlight html >}}
<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import $ from 'jquery'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  mounted() {
    $('h1').css('color', '#f72585')
    $('h3').css({'fontSize': '22px','color': '#4895ef'})
    $('a').css({'fontSize': '18px', 'fontWeight': 'bold','color': '#7209b7'})
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
{{< /highlight >}}


As we can see, changes are applied to DOM elements and we can use other functionalities of **JQuery** in our Vue component.

`http://localhost:8080/`

![vue-jquery-end-result](/images/vue-jquery-import/vue-jquery-end-result.png)



