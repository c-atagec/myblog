+++
title = "Load a Global Scss File in Vue 3"
date = "2021-07-04T11:19:00+03:00"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["scss","css","vue.config.js","vue-3","vue-3-project-setup","vue-cli-4","vuejs",]
+++


Loading scss into our Vue projects can be a little tricky. Especially with the webpack compatibility issues with `sass-loader`.

In this post we're going to learn how to use scss files globally in a **Vue 3** app using **Vue CLI 4**.

Before we move on, make sure to install [Node.js](https://nodejs.org/en/ "Node.js") on your system. Vue CLI 4 requires version 8.9 or above.

## Installing Vue CLI 4
Run the command below to install Vue CLI 4 globally.
{{< highlight sh >}}
npm install -g @vue/cli
{{< /highlight >}}

## Creating our Vue App
When Vue CLI 4 installation is finished, create a project called **my-app**. ( or whatever you prefer. )
{{< highlight sh >}}
vue create my-app
{{< /highlight >}}

- Select **Manually select features** preset

![selecting-setup-step-1](/images/vue-cli-4-fonts/vue-cli-setup-1.png)

- Deselect **Linter / Formatter** option ( you can use spacebar to deselect it )

![selecting-setup-step-2](/images/vue-cli-4-fonts/vue-cli-setup-2.png)

- Choose **Vue 3.x** as a version

![selecting-setup-step-3](/images/vue-cli-4-fonts/vue-cli-setup-3.png)

- Select your config to place in **In dedicated config files**

![selecting-setup-step-4](/images/vue-cli-4-fonts/vue-cli-setup-4.png)

- For the last step, select **No** and continue.

After your setup is complete you can go to your project folder, and type **npm run serve** to preview the Vue app. 

`http://localhost:8080/` 

![selecting-setup-step-5](/images/vue-cli-4-fonts/vue-cli-5.png)


## Folder Structure of the Project
After our application is alive and running, we can now create our ```colors.scss``` file under ```assets``` folder.

![folder-structure](/images/vue-global-scss/folder-structure-colors.png)


## Creating Color Variables 
In ```colors.scss``` file let's create variables for **red**, **green** and **black** colors.

{{< highlight css >}}
// RED
$red: #b22222;

// GREEN
$green: #76ff7a;

// BLACK
$black: #000;

{{</ highlight  >}}


## Installing sass and sass-loader
As default, Vue CLI 4 uses **webpack** 4 as a webpack version, which has compatibility issues with the latest **sass-loader** version. To work around that, we are going to install specific version of the **sass-loader**. You can also check the [docs](https://cli.vuejs.org/guide/css.html#pre-processors) for this issue.


{{< highlight sh >}}
npm install -D sass-loader@^10 sass
{{< /highlight >}}



## Creating vue.config.js File
With the help of vue.config.js file we can use our variables, mixins, fonts etc. all across our components. That way, we don't have to manually import necessary features into every single component.

Now, let's create one in the root folder of our application.

![vue-js-config-file](/images/vue-cli-4-fonts/vue-cli-vue-config.png)


## Configuring vue.config.js File

`vue.config.js`

{{< highlight js >}}
module.exports = {
  css: {
    loaderOptions: {
      scss: {
        additionalData: `@import "~@/assets/scss/colors.scss";`
      }
    }
  }
}
{{< / highlight >}}

Here, we set the internal configuration for the webpack loader, and loaded our  `colors.scss` file globally. That way, we can access it from each component in the application.

## Using Color Variables in a Component
In `App.vue` component our current styling is like this.

{{< highlight css >}}
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

By adding `lang="scss"` attribute to style tag, we can now use our stylings as below.

{{< highlight css >}}
<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1 {
  color: $red;
}

h3 {
  color: $green;
}

li {
  background-color: $black;
}

</style>

{{< /highlight >}}

## Restarting  Dev Server

Sometimes development server doesn't pickup the changes right away. Make sure to restart the dev server before checking the end result.

We now can see that stylings are applied to ``h1``, ``h3`` and ``li`` elements. 

![vue-cli-end-result](/images/vue-global-scss/global-scss-end-result.png)





