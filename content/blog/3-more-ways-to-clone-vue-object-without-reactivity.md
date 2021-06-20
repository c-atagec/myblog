+++
title = "3 More Ways to Clone Vue Object Without Reactivity"
date = "2021-06-16T08:01:45+03:00"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["block","css","display","google-fonts","inline","inline-block","local-fonts","none","vue-3","vue-3-project-setup","vue-cli-4","vuejs",]
+++

Hey, in the last post we looked at different ways to how to clone a Vue object and make it non-reactive. As I was searching for if there are more ways to do it,
I found this [blog post](https://dassur.ma/things/deep-copy/), written by Surma ( Web Advocate @Google ). 

Aside from some of the methods that mentioned in my [previous blog post](http://localhost:1313/blog/8-ways-to-clone-vue-object-and-make-it-non-reactive/), he talks about **structured cloning algorithm**, which nice thing about it is, it handles [cyclic objects.](https://riptutorial.com/javascript/example/14476/cyclic-object-values)

So, let's have a brief pause here to take a look at what structured cloning algorithm is.

Here's the definition from MDN.

> The structured clone algorithm copies complex ***JavaScript objects***. 
> It is used internally to transfer data between ***Workers*** via ***postMessage()***

Basically, it's an algorithm that allows us to clone JavaScript objects. But, as the MDN documentation [says](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Structured_clone_algorithm#supported_types), it works if your data is a plain object.

In his post, he goes over 3 methods that use structured cloning algorithm. They are:

1) Message Channel

2) History API

3) Notification API

Without further ado, let's see them in action.

## DISCLAIMER

These methods only work with **JSON objects**. Because, we serialize the data first ( with **JSON.parse(JSON.stringify())** ), and then are calling the function. 

Without serializing the data these methods will throw an error.

## Message Channel

Structured cloning algorithm is available inside ***postMessage*** method, and we can send copied objects to ourselves as a message.
This is an asynchronous method, bear in mind while using that.

```js 
function messageClone(sourceObj) {
  return new Promise(resolve => {
    // Get port1 and port from MessageChannel
    const { port1, port2 } = new MessageChannel();
    // Receive postMessage event and resolve the cloned object
    port2.onmessage = event => resolve(event.data);
    // Send sourceObj with postMessage
    port1.postMessage(sourceObj);
  });
}

const sourceObj = { item: "white", item2: "black", item3: "purple" };
const clonedObj = await messageClone(JSON.parse(JSON.stringify(sourceObj)));
```

## History API
With History API we can update the most recent entry on the history stack, and receive the cloned object with **history.state**.


```js
historyClone(sourceObj) {
  // assign the state at the top of history stack to a variable
  const oldState = history.state;
  // replace history state with sourceObj, document.title is used as a title parameter which is optional
  history.replaceState(sourceObj, document.title);
  // get cloned object from history.state 
  const copy = history.state;
  // replace sourceObj with oldState
  history.replaceState(oldState, document.title);
  // return cloned object
  return copy;
}

const sourceObj = {item: 'white', item2: 'black', item3: 'purple'}
const clonedObj = historyClone(JSON.parse(JSON.stringify(sourceObj)))
```

## Notification API
When the new notification is created with providing a data property, clone of the provided data can be accessed from the newly created notification's data property.

```js
notificationClone(sourceObj) {
  // Pass sourceObj as the notification's data 
  // and get cloned object from newly created notification's data property 
  return new Notification("", { data: sourceObj, silent: true }).data;
}

const sourceObj = {item: 'white', item2: 'black', item3: 'purple'}
const clonedObj = notificationClone(JSON.parse(JSON.stringify(sourceObj)))
```

## Conclusion
You can make use of these methods if it's met your certain scenario, and get a little bit more insight about cloning Vue objects without reactivity.
