+++
title = "How to Use VueDraggable With BootstrapVue's b-table"
date = "2021-10-10T11:44:07+03:00"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = ["bootstrap-vue","b-table","b-table-simple","vuejs-2", "sortablejs","vue-draggable", "vue-draggable-next"]
+++

## Introduction

Most of the time, we use multiple libraries and packages in our project and stumble upon a problem that has limited examples or the existing ones that don't work for our specific scenario, and there's a lack of information about using **VueDraggable** with **Bootstrap Vue**'s `b-table`.

By the end of this blog post, we are going to learn how to use **VueDraggable** with **Bootstrap Vue**'s `b-table`, and update the model as we made changes in the DOM.

Before we dive into the solution let's go over existing ones and examine why they don't work as expected.

## Examining Existing Solutions

With a quick search, you might come across this [GitHub issue](https://github.com/SortableJS/Vue.Draggable/issues/542) about Bootstrap Vue's integration with VueDraggable, which leads to this [StackOverflow question](https://stackoverflow.com/questions/54670693/draggable-table-with-bootstrap-vue/54683151)
then also leads to this [jsfiddle example](https://jsfiddle.net/karthickj25/btfmh9ap/)

The missing ingredient with this solution is that the data model is **not reacting** to the **changes** made in the DOM. It also uses **Sortable.js** library as a solution, which **falls short** on updating data model.

Check out [this example](https://jsfiddle.net/catagec/1mzn4ph5/) to see it in action.

So, let's take a closer look at why the data model is not affected.

## SortableJS approach ( and why it doesn't trigger data model to change )

By using SortableJS we can achieve the drag and drop functionality **on a DOM level**. But, since SortableJS uses pure JavaScript to **manually manipulate the DOM**, Vue **isn't aware** of these changes, hence the data model **doesn't update**.

`template`

```html
<div id="app">
  <b-table v-sortable="sortableOptions" striped hover :items="items" class="table-item"></b-table>
  <div class="items">
    <p class="model-header">Data Model</p>
    <pre>
        {{ formattedItems }}
      </pre
    >
  </div>
</div>
```

`script`

```js
const createSortable = (el, options, vnode) => {
  return Sortable.create(el, {
    ...options
  });
};

const sortable = {
  name: "sortable",
  bind(el, binding, vnode) {
    const table = el;
    table._sortable = createSortable(table.querySelector("tbody"), binding.value, vnode);
  }
};

var app = new Vue({
  el: "#app",
  directives: {
    sortable
  },
  data() {
    return {
      items: [
        {
          id: 1,
          name: "Abby",
          sport: "basketball"
        },
        {
          id: 2,
          name: "Brooke",
          sport: "soccer"
        },
        {
          id: 3,
          name: "Courtenay",
          sport: "volleyball"
        },
        {
          id: 4,
          name: "David",
          sport: "rugby"
        }
      ],
      sortableOptions: {
        chosenClass: "is-selected"
      }
    };
  },
  computed: {
    formattedItems() {
      const str = JSON.stringify(this.items, null, 2);
      return str;
    }
  }
});
```

![sortablejs-example](/images/bootstrap-vue-draggable/sortablejs-display.gif)

As we can see, when we drag and drop elements, the data model stays the same. Which is not what we want as a result.

Now, let's move on to the next solution to make it work.

## Using VueDraggable ( and why it triggers data model to change )

VueDraggable uses Sortable.js **underneath**, but since it is **written for the Vue**, our data model and the DOM always **stays in sync**.

Let's see how it works with an example. ( Just a heads up, we are using **VueJS2** in this example, for **Vue3** take a look at [VueDraggableNext](https://github.com/SortableJS/vue.draggable.next) )

`template`

```html
<div id="app">
  <b-table-simple class="table-item">
    <b-thead>
      <!-- You can dynamically generate column names 
            or manually create it -->

      <!-- Dynamic column Names -->
      <!-- <b-th v-for="(item, name) in items[0]" :key="name" >
          {{ name }}
        </b-th> -->

      <!-- Static column Names -->
      <b-th> Id </b-th>
      <b-th> Name </b-th>
      <b-th> Sport </b-th>
    </b-thead>

    <!-- Draggable rows -->
    <draggable :class="{ [`cursor-grabbing`]: drag === true }" v-model="items" group="items" @start="drag = true" @end="drag = false" tag="tbody">
      <b-tr v-for="item in items" :key="item.id" class="item-row">
        <b-td> {{ item.id }}</b-td>
        <b-td>{{ item.name }}</b-td>
        <b-td>{{ item.sport }}</b-td>
      </b-tr>
    </draggable>
  </b-table-simple>

  <!-- Display data model -->
  <div class="items">
    <p class="model-header">Data Model</p>
    <pre>
        {{ formattedItems }}
      </pre
    >
  </div>
</div>
```

```js
import draggable from "vuedraggable";
export default {
  name: "App",
  components: {
    draggable
  },
  data() {
    return {
      drag: false,
      items: [
        { id: 1, name: "Abby", sport: "basketball" },
        { id: 2, name: "Brooke", sport: "soccer" },
        { id: 3, name: "Courtenay", sport: "volleyball" },
        { id: 4, name: "David", sport: "rugby" }
      ]
    };
  },
  computed: {
    // Format data type to be displayed as JSON string
    formattedItems() {
      const str = JSON.stringify(this.items, null, 2);
      return str;
    }
  }
};
```

![vuedraggable-display](/images/bootstrap-vue-draggable/vuedraggable-display.gif)


As expected, when the position of the rows has **changed**, our data model gets **updated synchronously**.

At this point, you might ask, why are we using `b-table-simple` instead of `b-table` component, because currently, there's no workaround to *directly* integrate **VueDraggable** with b-table. 

You can check out this [GitHub issue](https://github.com/bootstrap-vue/bootstrap-vue/issues/873) for a detailed explanation. 


## Conclusion and live examples
Next time you use BootstrapVue with VueDraggable remember:

1) If you're using VueJS2 stick to VueDraggable, otherwise ( meaning using Vue3 ) use VueDraggableNext.

2) Use `b-table-simple` instead of `b-table` component.

Check out this [live example](https://codesandbox.io/s/bootstrap-vue-vue-draggable-example-k76ve?file=/src/App.vue) and use it as a template next time you integrate b-table-simple component with VueDraggable.

You can also take a look at this [Sortable.js demo](https://jsfiddle.net/catagec/4uLjco10/31/) to compare it with the working solution.

If you want me to create an example with Vue3 and VueDraggableNext [shoot me a DM](https://twitter.com/CAtagec), share your thoughts and ideas. I'm listening!