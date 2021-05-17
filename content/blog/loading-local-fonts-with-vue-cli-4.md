+++
title = "How to Load Local Fonts with Vue CLI 4 in Vue 3"
date = "2021-05-16T15:29:58+03:00"
description= "In this post we're going to learn how to use our local fonts in our Vue 3 application using Vue CLI 4"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["vuejs","vue-3","vue-cli-4","google-fonts", "local-fonts"]
+++


Using local fonts in our Vue projects can be a little tricky. Especially with the webpack compatibility issues with `sass-loader`.

In this post we're going to learn how to use our local fonts in our **Vue 3** application using **Vue CLI 4**.

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
After our application is alive and running, we can now create subfolders ( under the assets folder ) for font and styling files.

![folder-structure](/images/vue-cli-4-fonts/vue-cli-folder.png)

## Importing Local Fonts Into scss File
To load our fonts we are using @font-face [at-rule](https://developer.mozilla.org/en-US/docs/Web/CSS/At-rule).

`fonts.scss`
{{< highlight css >}}
@font-face {
  font-family: 'Karla-Regular';
  src: local('Karla-Regular'), url('~@/assets/fonts/Karla-Regular.ttf') format("truetype");
}

@font-face {
  font-family: 'Karla-Bold';
  src: local('Karla-Bold'), url('~@/assets/fonts/Karla-Bold.ttf') format("truetype");
}

{{< /highlight >}}

To give a brief explanation:
- Using `font family` property we are setting a name for the font itself.

- `local()` function searches user's computer if there's a local font with the name provided. If there's a match, that local font is used. If not, it looks for the resource specified in `url()` function.

- In the `url()` function we provide a path for our local fonts, `@` symbol is a reference to `src/` folder.

- Lastly, we are defining a format type in `format()`. Other available formats are, *woff*, *woff2*, *opentype*, *embedded-opentype*, and *svg*.



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
        additionalData: `@import "~@/assets/scss/fonts.scss";`
      }
    }
  }
}
{{< / highlight >}}

Here, we set the internal configuration for the webpack loader, and loaded our  `fonts.scss` file globally. That way, we can access it from each component in the application.

## Using Local Fonts in Vue Component
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

By adding `lang="scss"` attribute to style tag, we can now use our fonts as below.

We can also create separate **scss** file to use our fonts. Both is going to work.

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
  font-family: 'Karla-Bold';
  font-size: 40px;
  color: #9403fc;
}

h3 {
  font-family: 'Karla-Regular';
  font-size: 30px;
  color: #07c9e3;
}
</style>

{{< /highlight >}}

## Here's the End Result

After applying our styles, we now can see that our fonts are applied to **h1** and **h3** elements.  ( Make sure to restart your local server to see the changes. )

![vue-cli-end-result](/images/vue-cli-4-fonts/vue-cli-end-result.png)


## Conclusion

By configuring `css.loaderOptions` option in `vue.config.js` file we can use our shared styles throughout our application. It saves us from repeated work.


