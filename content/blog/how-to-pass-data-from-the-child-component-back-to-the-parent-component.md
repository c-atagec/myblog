+++
title = "How to Pass Data From the Child Component Back to the Parent Component"
date = "2021-10-28T16:17:02+03:00"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["vue-3", "$emit", "passing-data-between-components"]
+++

## Introduction

Sometimes in our Vue application, as the project gets bigger, we split our UI into multiple components, and we want to be able to pass data between these components.

In the scenario of passing data from parent to child, we can use **props,** but for passing data back to the parent from child component **props** is not the method that we want, for that we can use **$emit** method.

By the end of this blog post, you will learn how to pass data from child component to parent component using **$emit** method.

## What is Emit event?

In Vue, **$emit** is a way to pass data between the components. You can think of it like props but in reverse order. They're not exactly the same with usage, but we can conceptualize the process like this. Wherewith props we are passing the data **parent → child** direction, but with **$emit** the order becomes **child → parent.**

## How to Emit an Event from ChildComponent

Let's think of it this way. We have a display of products in our application.

![vue-emit-product-display.png](/images/vue-emit-assets/vue-emit-product-display.png)

And each **ProductItem.vue** has an **AddToCart** button. When the button is clicked the **ProductDisplay.vue** has to know that a new product is added to the cart and update the cart  accordingly.

![vue-emit-cart-update.png](/images/vue-emit-assets/vue-emit-cart-update.png)

For this, we can **$emit** an event from **child component ( ProductItem.vue )** and **listen** to that event in the **parent component ( ProductDisplay.vue )**

### Child component ( ProductItem.vue )

`template`

```html
<template>
  <div class="product-item column">
    <p class="product-title">{{ item.name }}</p>

    <img :src="item.imageUrl" :alt="item.name" />

    <span class="product-price">
      {{ item.price }}
    </span>
    <button type="button" class="add-to-cart-button" @click="addToCart(item)">
      Add To Cart
    </button>
  </div>
</template>
```

`script`

```jsx
<script>
export default {
  name: "ProductItem",
  props: {
    item: Object,
  },
  emits: ["add-to-cart"],
  data() {
    return {};
  },

  methods: {
    addToCart(item) {
      // Emit an event named "add-to-cart"
      this.$emit("add-to-cart");
    },
  },
};
</script>
```

### Parent component ( ProductDisplay.vue )

`template`

```html
<template>
  <div class="products-container row">
      
    <!--  Listen for the add-to-cart event -->
    <!-- Run addedToCart method when event is happened -->
    <ProductItem
      v-for="item in items"
      :key="item.id"
      :item="item"
      @add-to-cart="addedToCart"
    />
    
    <div class="cart-container">
      <span class="cart-title">Cart</span>
      <span class="cart-count">{{ cartCount }}</span>
    </div>
  </div>

</template>
```

`script`

```jsx
<script>
import Shoe1 from "../assets/shoe-1.jpg";
import Shoe2 from "../assets/shoe-2.jpg";
import Shoe3 from "../assets/shoe-3.jpg";

import ProductItem from "./ProductItem";
export default {
  name: "ProductDisplay",
  components: {
    ProductItem,
  },
  data() {
    return {
      items: [
        {
          id: 1,
          imageUrl: Shoe1,
          name: "Shoe 1",
          price: "$55",
        },
        {
          id: 2,
          imageUrl: Shoe2,
          name: "Shoe 2",
          price: "$65",
        },
        {
          id: 3,
          imageUrl: Shoe3,
          name: "Shoe 3",
          price: "$60",
        },
      ],
      cart: [],
    };
  },
  methods: {
    // Get payload data with destructure syntax
    addedToCart({ ...payload }) {
      // Add payload data to the cart
      this.cart.push(payload);
    },
  },
  computed: {
    cartCount() {
      return this.cart.length;
    },
  },
};
</script>
```

Let's have a pause and take a look at what **this.$emit** actually means. **this.$emit**, sends a signal named **add-to-cart**, and we can catch that signal from the parent component ( with **@add-to-cart**   ) and run **addedToCart** method.

![emit-diagram-updated.png](/images/vue-emit-assets/emit-diagram-updated.png)

So, let's see what happens when we click **addToCart** button. 

![vue-emit-dev-tools.png](/images/vue-emit-assets/vue-emit-dev-tools.png)

As expected we can observe our **emitted event** as well as the **payload** in Vue Developer Tools.

![vue-emit-cart-data.png](/images/vue-emit-assets/vue-emit-cart-data.png)

We can also see that our Cart data gets **updated** with **payload** that we get from our emitted event.

![vue-emit-added-to-cart.png](/images/vue-emit-assets/vue-emit-added-to-cart.png)

As a result, our view gets updated according to the data.

You can check out the [working example](https://codesandbox.io/s/vue-3-emit-parent-child-cwtbx) to see it in action.


## Emit Syntax

By default **$emit** takes a string as a first parameter, which is the name of the emitted event.

```jsx
this.$emit('add-to-cart')
```

As a second parameter, we can pass the data value we want to send to.

```jsx
this.$emit('add-to-cart', 'shoe-1')
```

The second parameter can be a `string`, an `integer`, an `array`, an `object` or a `variable`.

We can also send **multiple values** like this.

**Child**

```jsx
this.$emit('add-to-cart', 'shoe-1', '$55',2)
```

**Parent**

```jsx
addedToCart(...payload) {
		console.log("payload: ", payload); // payload: ['shoe-1', '$55',2]
}
```

`Array Syntax`

**Child**

```jsx
this.$emit('add-to-cart', ['shoe-1', '$55', 2])
```

**Parent**

```jsx
addedToCart([...payload]) {
		console.log("payload: ", payload); // payload: ['shoe-1', '$55',2]
}
```

`Object Syntax`

**Child**

```jsx
this.$emit('add-to-cart', {shoe: 'shoe-1', price: '$55', quantity: 2})
```

**Parent**

```jsx
addedToCart({...payload}) {
		console.log("payload: ", payload); // payload: ['shoe-1', '$55',2]
}
```



Thus far, we've learned how to emit data from a child component. But what if we have nested components that we want emit to an event from. Let's take a look at that scenario.


## Emitting event up a chain of elements ( Grandchild to Grandparent )

For example, in our application, we could have **addToCart** button as a separate component. So that we can use it throughout the application. 

![vue-emit-atc-separate.png](/images/vue-emit-assets/vue-emit-atc-separate.png)

- Instead of emitting '**add-to-cart'** event from **ProductItem.vue**

 ( from **Child** to **Parent** ) ( **ProductItem.vue** → **ProductDisplay.vue**),

 -  we can emit it from **<AddToCart />**

( from **Grandchild** to **Grandparent** ) ( **AddToCart.vue** → **ProductItem.vue** → **ProductDisplay.vue** );

Like this.

![vue-emit-grand-child.png](/images/vue-emit-assets/vue-emit-grand-child.png)

Here's the working code.

**AddToCart.vue**

`template`

```html
<template>
  <button type="button" class="add-to-cart-button" @click="buttonClicked">
    Add To Cart
  </button>
</template>
```

`script`

```jsx
<script>
export default {
  name: "AddToCart",
  methods: {
    buttonClicked() {
      this.$emit("button-clicked");
    },
  },
};
</script>
```

**ProductItem.vue**

`template`

```html
<template>
  <div class="product-item column">
    <p class="product-title">{{ item.name }}</p>

    <img :src="item.imageUrl" :alt="item.name" />

    <span class="product-price">
      {{ item.price }}
    </span>

    <AddToCart @button-clicked="addToCart(item)" />
  </div>
</template>
```

`script`

```jsx
<script>
import AddToCart from "./AddToCart";

export default {
  name: "ProductItem",
  components: {
    AddToCart,
  },
  props: {
    item: Object,
  },
  emits: ["add-to-cart"],
  data() {
    return {};
  },
  methods: {
    addToCart(item) {
      this.$emit("add-to-cart", item);
    },
  },
};
</script>
```

**ProductDisplay.vue**

`template`

```html
<template>
  <div class="products-container row">
    <ProductItem
      v-for="item in items"
      :key="item.id"
      :item="item"
      @add-to-cart="addedToCart"
    />
    <div class="cart-container">
      <span class="cart-title">Cart</span>
      <span class="cart-count">{{ cartCount }}</span>
    </div>
  </div>
</template>
```

`script`

```jsx
<script>
import Shoe1 from "../assets/shoe-1.jpg";
import Shoe2 from "../assets/shoe-2.jpg";
import Shoe3 from "../assets/shoe-3.jpg";

import ProductItem from "./ProductItem";
export default {
  name: "ProductDisplay",
  components: {
    ProductItem,
  },
  data() {
    return {
      items: [
        {
          id: 1,
          imageUrl: Shoe1,
          name: "Shoe 1",
          price: "$55",
        },
        {
          id: 2,
          imageUrl: Shoe2,
          name: "Shoe 2",
          price: "$65",
        },
        {
          id: 3,
          imageUrl: Shoe3,
          name: "Shoe 3",
          price: "$60",
        },
      ],
      cart: [],
    };
  },
  methods: {
    addedToCart({ ...payload }) {
      this.cart.push(payload);
      console.log("cart", this.cart);
    },
  },
  computed: {
    cartCount() {
      return this.cart.length;
    },
  },
};
</script>
```

And here's how it looks in action.

![vue-emit-atc.gif](/images/vue-emit-assets/vue-emit-atc.gif)

You can also check out the [code example](https://codesandbox.io/s/vue-3-emit-grandparent-grandchild-i5762)

## Important Distinction To Make

As we saw in the examples, **this.$emit** emits an event and we can listen to that event in **parent component.** A critical point to keep in mind is, that we are emitting an event in the child's **own instance,** we are **NOT** directly emitting an event to the parent component. Much like the radio signal, if we don't turn up the radio we are not catching the radio waves that play the music.

I know we talked about this concept but it's an important point to understand to grasp the inner workings of Vue.

## Emitting an event from deeply nested elements ( from Grand-Grand-Grand Child to Grand-Grand-Grand Parent )

Aside from the examples of this post, you might ask what if I have deeply nested items? How am I going pass data between them? 

Well, Sunil Sandhu explains this in [his blog post](https://www.telerik.com/blogs/how-to-emit-data-in-vue-beyond-the-vuejs-documentation) very clearly. To summarize:

1) We could use $emit **all the way up to the chain of elements** ( This method can get quite boring and it gets harder to control as nesting level increases )

2) We could use **VueX** to dispatch events from deeply nested elements. This approach provides us with more control over our **$emit** mechanism. Because of the state management system in **VueX** we can easily dispatch an event and catch the changes.

3) Another alternative is using **Global Event Bus.** You can think of it as creating your own state management system. However, Vue Core Team generally advises against using this method ( [see here](https://forum.vuejs.org/t/clarification-on-not-using-global-event-bus/21028) ), but you can research and evaluate the usage of this method in your own scenario.

## Conclusion and live examples

Understanding $emit event and using it to pass data between components is essential for expanding our Vue knowledge. 

You can check out these live examples to play around with the code. Remember, practice makes it perfect!

- [vue-emit-parent-child](https://codesandbox.io/s/vue-3-emit-parent-child-cwtbx?file=/src/components/ProductDisplay.vue)
- [vue-emit-grandparent-grandchild](https://codesandbox.io/s/vue-3-emit-grandparent-grandchild-i5762?file=/src/components/ProductDisplay.vue)

If you want to run the project in your local environment you can also check out these repositories.

- [repo-vue-emit-parent-child](https://github.com/c-atagec/vue-3-emit-parent-child)
- [repo-vue-emit-grandparent-grandchild](https://github.com/c-atagec/vue-3-emit-grandparent-grandchild)