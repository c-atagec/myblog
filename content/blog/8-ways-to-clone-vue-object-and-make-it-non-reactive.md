+++
title = "8 Ways to Clone Vue Object and Make It Non Reactive"
date = "2021-06-08T22:27:11+03:00"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["block","css","display","google-fonts","inline","inline-block","local-fonts","none","vue-3","vue-3-project-setup","vue-cli-4","vuejs",]
+++

Today, we are going to learn how to clone a reactive object into a non-reactive one, so we can use it in our application without worrying about its reactiveness.

Without further ado, let's jump right in.

**Table of Contents**


{{< table_of_contents >}}


## What is Shallow Clone?

If you have a primitive data type let's say, let x = 3, when you make a copy of it let y = x, that copy value ( 3 ) will point into a new memory address, which y variable pointing into.

Shallow clone is not going to create a real copy of reference data types.  ( Object, Array, Function )

## What is Deep Clone?

Deep clone simply duplicates all primitive and reference properties of the source object, and puts them into the target object. Cloned properties of the target object will point into a new memory address, which is going to be a real copy.

## Shallow Clone vs Deep Clone

If you have an object that consists of only primitive data types ( String, Number, Boolean, undefined, BigInt, Symbol ), then you can shallow clone it to remove its reactivity. 

Conversely, if you have an object consists of reference types, you can't shallow clone it to remove its reactivity because it's going to point to the same reference address in memory.

Think of it this way ( and excuse my terrific drawing skills ),  if you have an original key (an object that consists of reference types ) that belongs to car1 ( reference address in the memory ) if you make a copy of it ( shallow clone ) that key is still going to belong into car1 ( same reference address in the memory ).

In other words, car1 will react to the cloned key, which is not what we want.

![shallow-clone-visualized](/images/vue-reactive-object-clone/shallow-clone-car-1.png)


In that case, you should use a deep clone to remove the reactivity of your object. Which duplicates all properties of your object ( primitive and reference types ) and puts them into the target object, that target object will point into a new address in the memory.

![deep-clone-visualized](/images/vue-reactive-object-clone/deep-clone-car.png)

Now that we took a glance at what shallow clone and deep clone are, let's jump right into examples.

## Shallow Clone Methods

### Spread Operator

Using a spread operator is one of the easiest ways to clone our reactive object into a non-reactive one.

Object

```js
const originalData = {item1: "one", item2: "two", item3: 3}
const clonedData = {...originalData}
```

Array

```js
const originalData = ["one", 2, "three"]
const clonedData = [...originalData]
```

### Object.assign()

With ***Object.assign( target, ...sources )*** we pass **target** object as a first parameter and **source** object ( or objects, we can put multiple objects as a source parameter,[see here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign?retiredLocale=tr#parameters)), as a second parameter to achieve the expected result.

Object syntax

```js
const originalData = {item1: "one", item2: "two", item3: 3}
const clonedData = Object.assign({}, originalData)
```

Array syntax

<!-- ```js -->
{{< highlight js>}}
const originalData = ["one", 2, "three"]
const clonedData = Object.assign([], originalData)
{{< /highlight >}}

<!-- ``` -->

### slice() method

We can make use of slice method to return a **shallow clone** of our data **without mutating** it. This method only applies to **arrays**.

Array syntax

```js
const originalData = ["one", 2, "three"]
const clonedData = originalData.slice();
```

### Lodash library clone() method

Another way to copy our data is to use lodash library. This library is usually preferred for handling arrays, objects, numbers, strings, etc. operations and makes it easier to duplicate data.

Object syntax

```js
const originalData = {item1: "one", item2: "two", item3: 3}
const clonedData = clone(originalData)
```

Array syntax

```js
const originalData = ["one", 2, "three"]
const clonedData = clone(originalData)
```

### JQuery.extend() method

Another alternative is to use JQuery.extend() method, although it is not commonly used, you might still consider it in your workflow.

Object syntax

```js
const originalData = {item: "one", item2: "two", item3: "three"}
const clonedData = JQuery.extend({}, originalData)
```

Array syntax

```js
const originalData = ["one", "two", "three"]
const clonedData = JQuery.extend([], originalData)
```

## Deep Clone methods

### Lodash library cloneDeep() method

Lodash library has cloneDeep() method to copy complex objects. Additionally, it copies **with** existing methods that the data includes.

```js
const originalData = [
        { 
          shoe: "Nike", 
          colors: {color1: "red", color2: "black", color3: "white"}, 
          prices: [100, 125, 150], 
          logo: new Function(),
          stock: 1000 
        },
        { 
          shoe: "Adidas", 
          colors: {color1: "blue", color2: "green", color3: "pink"}, 
          prices: [110, 135, 160], 
          logo: new Function(), 
          stock: 1500 
        },
        { 
          shoe: "New Balance", 
          colors: {color1: "black", color2: "yellow", color3: "orange"}, 
          prices: [100, 115, 150], 
          logo: new Function(),
          stock: 1200 
        }
      ];

const clonedData = cloneDeep(originalData)
```

### JSON.parse(JSON.stringify()))

If you want to use native **JavaScript** method, you can use **JSON.parse(JSON.stringify())** method. But be warned, this approach is going to work as expected **if** your data consists of supported JSON data types. These are:

1. **String**
2. **Number**
3. **Boolean**
4. **Null**
5. **Object**
6. **Array**

Let's take a look at working example.

```js
const originalData = [
        { 
          shoe: "Nike", 
          colors: {color1: "red", color2: "black", color3: "white"}, 
          prices: [100, 125, 150], 
          stock: 1000, 
          warranty: true, 
          season: null  
        },
        { 
          shoe: "Adidas", 
          colors: {color1: "blue", color2: "green", color3: "pink"}, 
          prices: [110, 135, 160], 
          stock: 1000, 
          warranty: false, 
          season: null 
        },
        { 
          shoe: "New Balance", 
          colors: {color1: "black", color2: "yellow", color3: "orange"}, 
          prices: [100, 115, 150], 
          stock: 1000, 
          warranty: true, 
          season: null 
        }
      ];

const clonedData = JSON.parse(JSON.stringify(originalData));
console.log(clonedData)
```

![json-stringify-working-example](/images/vue-reactive-object-clone/json-stringify-working-console.png)

As we can see, **JSON.parse(JSON.stringify())** method works smoothly with supported data types.

For other data types, this approach could yield unexpected results.

Let's see it in an example.

First, we are setting a **Map** object to diversify our example.

```js
const mapInstance = new Map();
mapInstance.set({shoe: "Nike"}, 1000)
mapInstance.set( "stock", 1000)
mapInstance.set("logo", "blue")
```

This is what an experimental object looks like.

```js
const testObject = {
        item1: "ice cream", // String
        item2: 2,           // Number
        item3: false,       // Boolean
        item4: null,        // null
        item5: { item: "one", item2: "two", item3: "three" }, // Object
        item6: ["one", "two", "three"], // Array
        item7: new Set([1, 2, 3, 4]), // Set Object
        item8: undefined, // undefined
        item9: Infinity,  // Infinity
        item10: new Function(), // Function
        item11: new Date(), // Date Object
        item12: mapInstance, // Map Object
        item13: Symbol("mac and cheese") // Symbol
      };

```

Here's how it looks on a console.

![json-stringify-not-working-before](/images/vue-reactive-object-clone//json-stringify-not-working-before.png)

If we try to clone it with JSON.parse(JSON.stringify()) method.

```js
const clonedTest = JSON.parse(JSON.stringify(testObject))
console.log(clonedTest)
```

This is what we're going to get as a result.

![json-stringify-not-working-after](/images/vue-reactive-object-clone//json-stringify-not-working-after.png)

As we can see unsupported data types ignored completely or mutated unexpectedly.

JSON.parse(JSON.stringify()) method can be useful within certain scenarios, so use it with caution.

### JQuery.extend(deep) method

If we use **JQuery.extend()** method with giving true as the first parameter, it's going to deep clone the object.

Object syntax

```js
const originalData =  { 
                        shoe: "Nike", 
                        colors: {color1: "red", color2: "black", color3: "white"}, 
                        prices: [100, 125, 150], 
                        logo: new Function(), 
                        stock: 1000 
                      }

const clonedData = JQuery.extend(true, {}, originalData);
```

Array syntax

```jsx
const originalData = [
        { 
          shoe: "Nike", 
          colors: {color1: "red", color2: "black", color3: "white"}, 
          prices: [100, 125, 150], 
          logo: new Function(), 
          stock: 1000 
        },
        { 
          shoe: "Adidas", 
          colors: {color1: "blue", color2: "green", color3: "pink"}, 
          prices: [110, 135, 160], 
          logo: new Function(), 
          stock: 1500 
        },
        { 
          shoe: "New Balance", 
          colors: {color1: "black", color2: "yellow", color3: "orange"}, 
          prices: [100, 115, 150], 
          logo: new Function(), 
          stock: 1200 
        }
      ];

const clonedData = JQuery.extend(true, [], originalData);
```

### Conclusion

As you can see, there's plenty of ways to clone our reactive object into a non-reactive one. Use whichever works best for you.

Although, we can write a native script to deep clone our object. it's the topic of another post.

Thank you for reading this far! Have a nice day.
