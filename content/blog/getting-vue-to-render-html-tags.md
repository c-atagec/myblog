+++
title = "Getting Vue to Render HTML Tags"
date = "2021-06-22T23:17:25+03:00"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["block","css","display","google-fonts","inline","inline-block","local-fonts","none","vue-3","vue-3-project-setup","vue-cli-4","vuejs",]
+++


In this super short and quick post, we are going to learn how to render HTML tags inside data function as working HTML.

As always, let's jump right into the action.

For this example, we have a title parameter as a string that includes HTML tags.

```js
 data() {
    return {
      dessert: {
        title: 'the <strong>mega super ultra stupendous</strong><br> <em>ice cream sundae</em>'
      }
    }
  }
```

## Problem

If we render the title parameter using the "Mustache" syntax, HTML tags are going to show up as a string.

```html
<p>{{dessert.title}}</p>
```

![html-tag-not-rendered](/images/vue-html-tag-render/html-tag-not-rendered.png)

## Solution

Vue has a ```v-html``` directive to overcome this problem. Which will directly update the element's innerHTML.

```html
<p v-html="dessert.title"></p>
```

Now, HTML tags are going to render as expected.

![html-tag-rendered](/images/vue-html-tag-render/html-tag-rendered.png)


## BUT BEWARE
This usage of directly injecting HTML to element can lead to [XSS attacks](https://en.wikipedia.org/wiki/Cross-site_scripting) and can be very dangerous, as mentioned in the [docs.](https://v3.vuejs.org/api/directives.html#v-html)

What is that exactly mean? 
As stated from [MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Types_of_attacks#cross-site_scripting_xss), the browser cannot detect the malicious script, hence it gives access to cookies, session tokens, or other sensitive site-specific information, or lets the malicious script rewrite the HTML document.

For further information, you can check out the [docs](https://v3.vuejs.org/guide/security.html#injecting-html).

Additionally, you can watch this [video](https://www.youtube.com/watch?v=ns1LX6mEvyM&t=207s) from Web Dev Simplified to gain a little more knowledge about the topic.

## Workaround

You can use [vue-sanitize](https://www.npmjs.com/package/vue-sanitize) package to filter out unwanted HTML tags, this will allow you to only render HTML tags that you permit.


## Conclusion

If you're 100% sure that the provided HTML is safe, meaning that you're working on a static website, or you are the one creating the content, then you can consider using ```v-html``` directive.

Otherwise, be very cautious if you're working with sensitive data. ( username, password, credit card information, etc. )



