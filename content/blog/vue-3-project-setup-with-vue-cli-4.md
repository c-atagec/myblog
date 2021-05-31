+++
title = "Vue 3 Project Setup With Vue CLI 4"
date = "2021-05-29T21:51:05+03:00"
description="Let's get up and running with our first Vue 3 project with Vue CLI 4"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["vue-3-project-setup","vue-3","vue-cli-4","vuejs",]
+++

In this post, we are going to setup our Vue 3 project with Vue CLI 4. Let's get started.

Just a heads up. Vue CLI 4 requires [Node.js](https://nodejs.org/en/ "Node.js") version 8.9 or above. Make sure to install it on your system.

## First step, installing Vue CLI 4
Use the command below to install Vue CLI 4 globally.

{{< highlight sh >}}
npm install -g @vue/cli
{{< /highlight >}}

## Second step, creating our first Vue project
Let's create our first project.
{{< highlight sh >}}
vue create first-project
{{< /highlight >}}

## Third step, selecting our project configuration

- Select **Manually select features** preset

![selecting-setup-step-1](/images/vue-cli-4-fonts/vue-cli-setup-1.png)

- Deselect **Linter / Formatter** option ( you can use spacebar to deselect it )

![selecting-setup-step-2](/images/vue-cli-4-fonts/vue-cli-setup-2.png)

- Choose **Vue 3.x** as a version

![selecting-setup-step-3](/images/vue-cli-4-fonts/vue-cli-setup-3.png)

- Select your config to place in **In dedicated config files**

![selecting-setup-step-4](/images/vue-cli-4-fonts/vue-cli-setup-4.png)

- For the last step, select **No** and continue.

## Last step, serving our project locally

After the project setup is completed, let's preview our Vue 3 app locally.

Go to your project folder.
{{< highlight sh >}}
cd first-project
{{< /highlight >}}

Run the development server

{{< highlight sh >}}
npm run serve
{{< /highlight >}}

Now, our app is live on 8080 port.

`http://localhost:8080/` 

![selecting-setup-step-5](/images/vue-cli-4-fonts/vue-cli-5.png)



