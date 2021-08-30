+++
title = "How to Use Router Link With Vuetify Buttons"
date = "2021-08-11T21:54:52+03:00"
og_image = "/images/vue-router-link-vuetify/setting-up-the-project.png"

#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["block","css","display","google-fonts","inline","inline-block","local-fonts","none","scss","vue-3","vue-3-project-setup","vue-cli-4","vue.config.js","vuejs",]
+++

If you've used vue-router and Vuetify in the same project you probably noticed that there's no connective explanation about how to use **router-link** with **Vuetify** buttons. 

The vue-router documentation says we can use **tag** prop to specify which tag to render to. But when we use this approach to render **v-btn** inside **router-link**, we can't pass the attributes we want and a double formatting issue arises.

Alternatively, when we don't use any tag attribute at all, vue-router wraps the entire button in a ``<a>`` tag. But then formatting for the button gets mutated and there'll be an underline added to the text as text-decoration.


By the end of this blog post, we will learn how to use **Vuetify** buttons ( with its attributes ) with **vue-router**, without wrapping it inside **router-link**. Also, we will take a look at unwanted scenarios and inspect why they aren't working.

### Project Setup

We are going to use **Vue CLI ( 4.5.13 )** to set up the project. This will allow us to add Vuetify with just 15 characters. So, let's get started.

### Creating the app

```jsx
vue create vuetify-router-buttons
```

### Project configuration

We will manually select features for this project.


![setting-up-the-project](/images/vue-router-link-vuetify/setting-up-the-project.png)

Just pick **Router** and continue to select **Vue version**. We don't need anything extra to demonstrate the example.

![/selecting-features](/images/vue-router-link-vuetify/selecting-features.png)

Choose **Vue 2.x** as a version.

![selecting-vue-version](/images/vue-router-link-vuetify//selecting-vue-version.png)

At this point, you might ask why are we choosing **Vue 2** for this project. Is there a specific reason? 

The answer is yes, here's a [direct explanation](https://vuetifyjs.com/en/getting-started/installation/#vue-cli-install) from Vuetify documentation.

> The current version of Vuetify **does not support Vue 3**. Support for Vue 3 will come with the release of Vuetify v3. When creating a new project, **please ensure** you selected **Vue 2** from the Vue CLI prompts, or that you are installing to an existing Vue 2 project.


For the next step, say **yes** to the using history mode for the router.

![selecting-history-mode](/images/vue-router-link-vuetify/selecting-history-mode.png)

Pick **dedicated config files** for project config, or whichever you prefer.

![setting-up-project-5](/images/vue-router-link-vuetify/setting-up-project-5.png)

If you are going to use this setup for future projects, you can save it as a preset or you can say **no** to this question if you're just experimenting.

![setting-up-project-6.png](/images/vue-router-link-vuetify/setting-up-project-6.png)

When setup is finished go into the project folder

```jsx
cd vuetify-router-buttons
```

### Installing Vuetify

Now that we have the initial setup completed, let's add Vuetify to our project.

```jsx
vue add vuetify
```

It's going to ask for an installation preset. Pick **Default (recommended)**

![setting-up-project-7.png](/images/vue-router-link-vuetify/setting-up-project-7.png)

When the installation is finished `npm run serve` to run the project.

![vuetify-project-loaded](/images/vue-router-link-vuetify/vuetify-project-loaded.png)

Now that we can see that Vuetify automaticallythe  adjusted project structure for our ease of use. This way we don't have to manually import the package into our components.

### Adjusting Project Structure

Here's the current structure of the project

![project-structure-before](/images/vue-router-link-vuetify/project-structure-before.png)

Let's remove **Home.vue, About.vue** and **Helloworld.vue** files, and in **views** folder create **Profile.vue, Feed.vue** and **Index.vue** files.

Here's how folder structure looks like after creating view files.

![project-structure-after](/images/vue-router-link-vuetify/project-structure-after.png)

Let's add some content to our newly created files.

**Feed.vue**

```jsx
<template>
  <div class="feed">
    <h3 class="display-3 text-center">Feed View</h3>
  </div>
</template>

<script>
export default {
  name: 'Feed'
}
</script>
```

**Index.vue**

```jsx
<template>
  <div class="index">
    <h3 class="display-3 text-center">Index View</h3>
  </div>
</template>

<script>
export default {
  name: 'Index'
}
</script>
```

**Profile.vue**

```jsx
<template>
  <div class="profile">
    <h3 class="display-3 text-center">Profile View</h3>
  </div>
</template>

<script>
export default {
  name: 'Profile'
}
</script>
```

Let's add our views into the **router/index.js**

```jsx
import Feed from "../views/Feed.vue";
import Index from "../views/Index.vue";
import Profile from "../views/Profile.vue";
```

```jsx
const routes = [
  {
    path: "/feed",
    name: "Feed",
    component: Feed
  },
  {
    path: "/index",
    name: "Index",
    component: Index
  },
  {
    path: "/profile",
    name: "Profile",
    component: Profile
  }
];
```

Lastly, let's change **App.vue** component

**App.vue**

```html
<template>
  <v-app>
    <v-main>
      <v-container>
        <div class="link-container d-flex justify-center align-center">
          <v-btn class="mx-4 white--text" elevation="2" x-large rounded color="deep-purple darken-1" to="/feed">
            Feed
          </v-btn>

          <v-btn class="mx-4 white--text" elevation="2" x-large rounded color="deep-purple darken-1" to="/profile">
            Profile
          </v-btn>

          <v-btn class="mx-4 white--text" elevation="2" x-large rounded color="deep-purple darken-1" to="/index">
            Index
          </v-btn>
        </div>
        <router-view />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data: () => ({
    //
  })
};
</script>

<style>
.link-container {
  padding: 5rem;
}
</style>
```

By using [to](https://vuetifyjs.com/en/api/v-btn/#api-props) which **v-btn** component provides, we don't have to wrap **router-link** around **v-btn buttons**.

Here's how it looks after the changes we made.

![end-result](/images/vue-router-link-vuetify/end-result.gif)


Now that we've come to an understanding using **vue-router** with **Vuetify**. We can now take a look at other scenarios and examine why they aren't working.


### Scenario 1: Router Link tag approach
We can tell **router-link** to render **v-btn** with it's [tag](https://router.vuejs.org/api/#tag) attribute. As a result, 2 buttons are going to be rendered, one inside another, and this will cause double formatting to be applied to the button.


``Code``

```html

<router-link to="/feed" tag="v-btn">
  <v-btn class="mx-4 white--text" elevation="2" x-large rounded color="deep-purple darken-1">Feed</v-btn>
</router-link>
```

``Rendered HTML`` 
```html
<button type="button" class="v-btn v-btn--is-elevated v-btn--has-bg theme--light v-size--default">
  <span class="v-btn__content">
    <button type="button" class="mx-4 white--text v-btn v-btn--is-elevated v-btn--has-bg v-btn--rounded theme--light elevation-2 v-size--x-large deep-purple darken-1">
      <span class="v-btn__content">Feed</span>
    </button>
  </span>
</button>
```

*Outer button* element applies extra styling and renders another button with **white background** and **box-shadow** around *inner button*.

![scenario-1-close-up](/images/vue-router-link-vuetify/scenario-1-exp.png)

### Scenario 2: Router Link tagless approach
We can directly nest **v-btn** inside **router-link**. without defining any tags.

``Code``
```html
<router-link to="/profile">
  <v-btn class="mx-4 white--text" elevation="2" x-large rounded color="deep-purple darken-1" to="/profile">Profile</v-btn>
</router-link>
```
``Rendered HTML``
```html
<a href="#/profile" class="">
  <a href="#/profile" class="mx-4 white--text v-btn v-btn--is-elevated v-btn--has-bg v-btn--rounded v-btn--router theme--light elevation-2 v-size--x-large deep-purple darken-1">
    <span class="v-btn__content">Profile</span>
  </a>
</a>
```

![scenario-2-close-up](/images/vue-router-link-vuetify/scenario-2-close-up.png)

These approaches won't give us desired results in the end, and sometimes they can be overlooked while taking care of the other 10 things in the project.

While using **v-btn** in your project, make sure to check [v-btn-api](https://vuetifyjs.com/en/api/v-btn/) to see other options you could use.

## Conclusion and Live Demo

To get a better understanding of how this actually works in the code, check out this [codepen](https://codepen.io/hunata/pen/rNmPoBy?editors=1010) I created, it includes all of the examples we've covered, and if you want to see more examples with Vuetify, [shoot me a DM](https://twitter.com/CAtagec) I'll be happy to hear from you.

And remember, the next time you are using **vue-router** with **Vuetify** in your project, simply add **to** prop to **v-btn** to avoid the hassle with **router-links**.

