## How to run the app
`npm run preview`

<hr>


# Update our knowledge, where to start?
## How to study Vue 3 new features
My students often ask me how to keep their knowledge up to date after they complete the Boolean Careers course.
This article aims to be a simple example of how to study new arguments.
First, it is necessary to read technical articles daily, and also to write code. Be careful, however it is essential not to fall into the tutorial hell!

In general, tutorials show us how to do a specific task and not the theory behind it, so if we want to understand the real mechanism, we have to create something ourselves.

### Let's start! Study Vue 3 Composition Api
Assuming you are familiar with Vue 2 - the most important new feature of Vue 3 is the **Composition Api**.

[## API Styles Reference](https://vuejs.org/guide/introduction.html#api-styles)

While the **Options Api** works similarly to Vue 2, the **Composition Api** works by importing Api functions into the logic of the component.

#### When using Composition Api?
From [Vuejs Doc](https://vuejs.org/guide/introduction.html#which-to-choose)

> The Composition API is centered around declaring reactive state variables directly in a function scope, and **composing state from multiple functions together to handle complexity**. It is more free-form, and requires understanding of how reactivity works in Vue to be used effectively. In return, its flexibility enables more powerful patterns for organizing and reusing logic.

> For production use:
> - Go with Options API if you are not using build tools, or plan to use Vue primarily in low-complexity scenarios, e.g. progressive enhancement.
> - Go with Composition API + Single-File Components if you plan to build full applications with Vue.

## Jump into Reactivity Fundamentals 
Now we have an overview of the Composition Api and we know when to use it, but we don't know yet how to use the Vue primary functionality:

### Reactivity
Start always from the documentation:
[Reactivity Fundamentals Reference](https://vuejs.org/guide/essentials/reactivity-fundamentals.html)

```JS
import { reactive } from 'vue'

const state = reactive({ count: 0 })
```

With the Composition Api we have to import the function we want to use in the script, in this case `reactive`, then we’ll use it to create a reactive state. This function accepts an object and returns a Proxy object.

From the documentation
> Reactive objects are **JavaScript Proxies** and behave just like normal objects. The difference is that Vue is able to track the property access and mutations of a reactive object.


##### What is a `Proxy` object?
[Proxy MDN reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) 

*In the previous version of Vue, instead, the reactivity was based on Getter and Setter.*
[Reactivity Vuejs 2](https://v2.vuejs.org/v2/guide/reactivity.html)

> When you pass a plain JavaScript object to a Vue instance as its data option, Vue will walk through all of its properties and convert them to getter/setters using **Object.defineProperty**

##### What is `Object.defineProperty`?
[Object.defineProperty MDN Reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

#### Go back to Vuejs Documentation
Now that we know we need to import the reactive function into our script, what should we do next?

> To declare a state in a component's template, we can use the `setup()` function.

```JS
import { reactive } from 'vue'

export default {
  setup() {
    const state = reactive({ count: 0 })

    // expose the state to the template
    return {
      state
    }
  }
}
```

> We can also declare functions in the same scope and expose them alongside the state.

```JS
import { reactive } from 'vue'

export default {
  setup() {
    const state = reactive({ count: 0 })

    function increment() {
      state.count++
    }

    // don't forget to expose the function as well.
    return {
      state,
      increment
    }
  }
}
```

> Because expose state and methods via the `setup()` function can be verbose, we can use in single file components the `<script setup>`

```JS
<script setup>
import { reactive } from 'vue'

const state = reactive({ count: 0 })

function increment() {
  state.count++
}
</script>
```
```HTML
<template>
  <button @click="increment">
    {{ state.count }}
  </button>
</template>
```

> Top-level imports and variables declared in `<script setup>` are automatically usable in the template of the same component.

### We are ready to: a bunch of bubbles!
Now we'll have to imagine a real situation to use Vuejs reactivity.
**You need to be creative in this phase**, don't use a typical example. Instead, **use an argument you like**.

As everyone knows, I love colors, so for this example, I have imagined a bunch of colored random sized bubbles and a set of baskets of the same color.

Our script will allow us to move the colored bubbles within the relative basket with a click, and with another click, we can send the bubble from the basket to the top of the bunch.
Finally, we can change the color of bubbles by dragging and dropping a colored tab on a bubble.

![Screen 1](./screens/Schermata%202022-03-22%20alle%2018.14.22.png)

## Where to start?
In this situation, we'll use a reactive `state` with 3 properties `bubbles`, `baskets`, `availableColors`.
Then we'll create the necessary methods.

We'll use the `reactive()` function in the `<script setup>`.

#### First step: the State
```JS
const state = reactive({
    bubbles: [
      {
        name: 'blue',
        color: '0,0,255',
        diameter: 2.2,
      },
      {
        name: 'pink',
        color: '164, 78, 161',
        diameter: 1.7,
      },
      {
        name: 'green',
        color: '0, 128, 0',
        diameter: 1.9,
      },
      {
        name: 'red',
        color: '255, 0, 0',
        diameter: 3,
      },
      {
        name: 'red',
        color: '255, 0, 0',
        diameter: 2.2,
      },
      {
        name: 'pink',
        color: '164, 78, 161',
        diameter: 1.9,
      },
      {
        name: 'green',
        color: '0, 128, 0',
        diameter: 3,
      },
      {
        name: 'blue',
        color: '0,0,255',
        diameter: 3,
      },
      {
        name: 'pink',
        color: '164, 78, 161',
        diameter: 2.8,
      },
      {
        name: 'red',
        color: '255, 0, 0',
        diameter: 2,
      },
      {
        name: 'green',
        color: '0, 128, 0',
        diameter: 1.4,
      },
      {
        name: 'red',
        color: '255, 0, 0',
        diameter: 2.8,
      },
      {
        name: 'green',
        color: '0, 128, 0',
        diameter: 2.6,
      },
      {
        name: 'blue',
        color: '0,0,255',
        diameter: 2,
      },
      {
        name: 'pink',
        color: '164, 78, 161',
        diameter: 2.5,
      },
      {
        name: 'green',
        color: '0, 128, 0',
        diameter: 1.4,
      },
      {
        name: 'pink',
        color: '164, 78, 161',
        diameter: 1.6,
      },
      {
        name: 'blue',
        color: '0,0,255',
        diameter: 2,
      },
      {
        name: 'red',
        color: '255, 0, 0',
        diameter: 1.5,
      },
    ],
    baskets: [{
        name: 'blue',
        color: '0,0,255',
        bubbles: []
      },
      {
        name: 'red',
        color: '255, 0, 0',
        bubbles: []
      },
      {
        name: 'green',
        color: '0, 128, 0',
        bubbles: []
      },
      {
        name: 'pink',
        color: '164, 78, 161',
        bubbles: []
      },
    ],
    availableColors: [{
        name: 'red',
        color: '255, 0, 0',
      },
      {
        name: 'blue',
        color: '0,0,255',
      },
      {
        name: 'green',
        color: '0, 128, 0',
      },
      {
        name: 'pink',
        color: '164, 78, 161',
      },
    ],
  });
```


#### Second Step: Template
Create a basic layout, we have: 
* `header`, with instructions,
* `.palette` that contains the available colors tabs,
* `.container` that contains our bubbles and relative baskets.
We pass some custom properties to css through the `style` attribute in order to handle the color and diameter of the bubble.

```JS
<li
          v-for="(bubble, indexBubble) in state.bubbles"
          :key="indexBubble + bubble.name"
          class="bubble"
          :style="
            `--diameter: ${bubble.diameter}em; 
             --color: ${bubble.color}
            `"
			/>
```

```HTML
<template>
  <header>
    <h2>A bunch of bubbles!</h2>
    <ol class="instructions">
      <li>Click on bubbles in stack to store in relative basket</li>
      <li>Click on bubbles in basket you send it to stack top</li>
      <li>Drag color on bubble to change its color</li>
    </ol>
  </header>
  <div class="container">
    <div class="stack">
      <ul
        v-if="state.bubbles.length > 0"
        class="stack-list"
      >
        <li
          v-for="(bubble, indexBubble) in state.bubbles"
          :key="indexBubble + bubble.name"
          class="bubble"
          :style="
            `--diameter: ${bubble.diameter}em; 
             --color: ${bubble.color}
            `"
        />
      </ul>
    </div>
    <div
      v-if="state.baskets.length > 0"
      class="baskets"
    >
      <ul class="baskets-list">
        <li
          v-for="(basket, indexBasket) in state.baskets"
          :key="indexBasket + basket.name"
          class="basket"
          :style="`--basket-color:${basket.color}`"
        >
          <ul class="content">
            <li
              v-for="(bubble, indexBubble) in basket.bubbles"
              :key="indexBubble + bubble.color"
              class="bubble"
              :style="
                `--diameter: ${bubble.diameter}em; 
                --color: ${bubble.color}
                `"
            />
          </ul>
        </li>
      </ul>
    </div>
  </div>
  <div class="palette">
    <ul>
      <li
        v-for="(color, index) in state.availableColors"
        :key="index + color.name+'-palette'"
        :style="`--palette-color: ${color.color}`"
      >
        {{ color.name }}
      </li>
    </ul>
  </div>
</template>

<style lang="scss">
  :root {
    --diameter: 1em;
    --color: 'grey';
    --basket-color: 'grey';
    --palette-color: 'grey';
  }

  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  #app {
    width: 100%;
    min-width: 1024px;
    height: 100vh;
    min-height: 500px;
    color: #2c3e50;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;

    header {
      height: 20%;
      padding: 1em 3em;
      background-color: rgb(216, 216, 216);
      font-size: 0.8em;
      .instructions {
        padding:  1em 1em 0;
      }
    }

    .container {
      display: flex;
      justify-content: space-around;
      align-items: flex-end;
      width: 70%;
      height: 70%;
      margin: 0 auto;
      padding: 5% 0;

      .stack {
        display: flex;
        flex-direction: row-reverse;
        align-items: flex-end;
        width: 17%;
        height: 100%;
        min-width: 100px;

        &-list {
          display: flex;
          justify-content: center;
          align-items: flex-end;
          gap: 0.2em;
          flex-wrap: wrap;
          list-style: none;
          font-size: 1rem;
        }
      }

      .baskets {
        width: 82%;
        height: 100%;

        &-list {
          display: flex;
          flex-direction: column;
          justify-content: flex-end;
          align-items: center;
          flex-wrap: wrap;
          width: 100%;
          height: 100%;
          list-style: none;
          font-size: 1rem;

          .basket {
            position: relative;
            display: flex;
            align-items: flex-end;
            flex-grow: 0;
            width: calc(100% / 6);
            min-width: calc(100% / 6);
            min-height: 100%;
            margin: 0.3em;
            border-radius: 1em;
            border-left: 2px inset rgba(var(--basket-color), 0.2);
            border-right: 2px inset rgba(var(--basket-color), 0.2);
            border-bottom: 10px inset rgba(var(--basket-color), 0.2);
            background: linear-gradient(to left, rgba(var(--basket-color), 0.3) 30%, rgba(var(--basket-color), 0.1) 60%, rgba(var(--basket-color), 0.2) 80%, rgba(var(--basket-color), 0.4) 90%, rgba(var(--basket-color), 0.2) 95%);
            text-align: center;

            &::after {
              content: "";
              position: absolute;
              bottom: -1em;
              left: 50%;
              transform: translateX(-50%);
              display: block;
              height: 1em;
              width: 130%;
              border-radius: 50%;
              background: radial-gradient(circle at center, rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0));
            }

            &::before {
              content: "";
              position: absolute;
              bottom: -0.8em;
              left: 50%;
              transform: translateX(-50%);
              display: block;
              height: 0.5em;
              width: 100%;
              border-radius: 50%;
              background: radial-gradient(circle at center, rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0));
            }

            .content {
              display: flex;
              flex-direction: column-reverse;
            }

            .bubble {
              margin: 0.4em;
            }
          }
        }
      }

      .bubble {
        position: relative;
        display: inline-block;
        width: var(--diameter);
        height: var(--diameter);
        border-radius: 100%;
        border: 1px inset rgba(var(--color), 0.3);
        background: radial-gradient(farthest-side at 0.6em 0.6em, rgba(var(--color), 0.3) 27%, rgba(var(--color), 0.5) 60%, rgba(var(--color), 0.7) 90%, rgba(var(--color), 0.8) 80%);
        cursor: pointer;

        &::after {
          content: "";
          position: absolute;
          bottom: 15%;
          right: 20%;
          display: block;
          width: 0.3em;
          height: 0.3em;
          border-radius: 100%;
          background: radial-gradient(rgba(255, 255, 255, 0.5) 20%, rgba(var(--color), 0.3));
        }
      }
    }

    .palette {
      height: 10%;
      width: 100%;
      background-color: rgba(0, 0, 0, 0.2);
      
      ul {
        display: flex;
        gap: 1em;
        justify-content: center;
        flex-wrap: wrap;
        padding: 0.8em;
        list-style: none;

        li {
          display: flex;
          justify-content: center;
          align-items: center;
          width: 6em;
          height: 100%;
          border: 1px inset rgba(var(--palette-color), 1);
          background: radial-gradient(farthest-side at 0.6em 0.6em, rgba(var(--palette-color), 1),  rgba(255, 255, 255, 0.7));
          border-radius: 2em;
          font-weight: bold;
          color: white;
          clip-path: circle(100%);
        }
      }
    }
  }
</style>
```

Now use (**Vuejs Devtools**)[https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd] and select the `App component` to see the `setup content`, it reflects our state (Reactive) and its array properties.

Try now to change the property color of one `bubbles` element, the color will change also in the browser: **this is the reactivity at work!**

![Vuejs Devtools](./screens/Schermata%202022-03-22%20alle%2018.18.52.png)

#### Third Step: methods
**`toBasket(index)`**

Clicking on a bubble we have to remove it from the `state.bubbles` and add it to the relative `state.basket`.

```JS
<script setup>
...

function toBasket(index) {
    //we remove the element from stack
    const [bubble] = state.bubbles.splice(index, 1);

    //we add bubble to relative basket
    state.baskets.forEach(element => {
      if(element.name == bubble.name) {
        element.bubbles.push(bubble);
      }
    });
}

...
</script>
```

In the `.stack-list>li` add `@click` handler

```HTML
<div
      v-if="state.bubbles.length > 0"
      class="stack"
    >
      <ul class="stack-list">
        <li
          v-for="(bubble, indexBubble) in state.bubbles"
          :key="indexBubble + bubble.name"
          class="bubble"
          :style="
            `--diameter: ${bubble.diameter}em; 
             --color: ${bubble.color}
            `"
          @click="toBasket(indexBubble)" 
        />
      </ul>
    </div> 
```

Ok, now if we click on a bubble it disappears from the left and appears in the relative basket on the right.

![Screen 2](./screens/Schermata%202022-03-22%20alle%2018.14.53%201.png)

#### Let's see **Vue Dev Tools**
![Vue Dev Tools](./screens/Schermata%202022-03-22%20alle%2018.22.44.png)
Now the `state.bubbles` array have 18 elements and the `state.baskests[0].bubbles` have 1 element.

#### Add `toTopStack()` function
`toTopStack()` will have to remove the bubble from a basket and add it on the stack's top when we click on a bubble.

```JS
function toTopStack(indexBasket,indexBubble) {
    //we remove the element from basket
    const [bubble] = state.baskets[indexBasket].bubbles.splice(indexBubble, 1);
    //we add element on top of bubbles
    state.bubbles.unshift(bubble);
}

```

```HTML
...

<div
      v-if="state.baskets.length > 0"
      class="baskets"
    >
      <ul class="baskets-list">
        <li
          v-for="(basket, indexBasket) in state.baskets"
          :key="indexBasket + basket.name"
          class="basket"
          :style="`--basket-color:${basket.color}`"
        >
          <h3>
            {{ basket.name }}
          </h3>
          <ul class="content">
            <li
              v-for="(bubble, indexBubble) in basket.bubbles"
              :key="indexBubble + bubble.color"
              class="bubble"
              :style="
                `--diameter: ${bubble.diameter}em; 
                --color: ${bubble.color}
                `"
              			@click="toTopStack(indexBasket,indexBubble)" 
            />
          </ul>
        </li>
      </ul>
    </div>
...
```

### Click event is very simple, add a little pepper! Drag and Drop API

![Drag drop](./screens/Schermata%202022-03-22%20alle%2018.24.56.png)

**What is HTML 5  drag and drop API?**
These are good places to start:
[# HTML Drag and Drop API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API)

[# Using the HTML5 Drag and Drop API](https://web.dev/drag-and-drop/?gclid=CjwKCAjwxOCRBhA8EiwA0X8hi6v5K5f4OR98lH6Mmtg9uda79z2Srx7uwQ7EKtGlW4o10IeGZt8eSRoCgiQQAvD_BwE)

Go on, put the attribute `draggable="true"` to the `.palette>li` and the handler `@dragstart` which calls our method `setColor($event, color)`, we pass it the event and the color object itself.

With these attributes, `.palette>li` will become draggable and when we start to drag the `setColor` method will be called.

On the `.stack-list>li` put 
```          
	@dragover.prevent
  @dragenter.prevent
  @drop="changeColor($event, indexBubble)"
```

We prevent the default behaviors `dragover` and `dragenter` end call on `drop` event our method `changeColor($event, indexBubble)`.
These methods accept the event and the index of bubble, that we use to search the right element in the `state.bubbles` array.

```HTML
  <div class="container">
    <div
      v-if="state.bubbles.length > 0"
      class="stack"
    >
      <ul class="stack-list">
        <li
          v-for="(bubble, indexBubble) in state.bubbles"
          :key="indexBubble + bubble.name"
          class="bubble"
          :style="
            `--diameter: ${bubble.diameter}em; 
             --color: ${bubble.color}
            `"
          @click="toBasket(indexBubble)" 
          @dragover.prevent
          @dragenter.prevent
          @drop="changeColor($event, indexBubble)"
        />
      </ul>
    </div> 
	  
	...
	  
	<div class="palette">
      <ul>
        <li
          v-for="(color, index) in state.availableColors"
          :key="index + color.name+'-palette'"
          :style="`--palette-color: ${color.color}`"
          draggable="true"
          @dragstart="setColor($event, color)"
        >
          {{ color.name }}
        </li>
      </ul>
    </div>
```

Now we use the `dataTransfer` object to set the drop effect and the allowed effect, as well as the data to be transferred by choosing its name.

```JS
function setColor(e, color) {
	e.dataTransfer.dropEffect = 'move';
	e.dataTransfer.effectAllowed = 'move';
	//we store object data in a JSON
	e.dataTransfer.setData('color', JSON.stringify(color));
}

function changeColor(e, bubbleIndex) {
	//And we parse JSON, we again have access to properties color and name 
	const color = JSON.parse(e.dataTransfer.getData('color'));
	// State is deeply reactive by default. 
	// We can also nested objects or arrays
	state.bubbles[bubbleIndex].name = color.name;
	state.bubbles[bubbleIndex].color = color.color;
}
```

#### Why do we use `JSON.stringify`?
We can transfer different types of data, including plain text, URLs, HTML code, files, etc.
In this case, we want to transfer an object literal, so we can use `JSON` object to` Stringify`, transform the object in a JSON formatted string, and then `Parse`,  transform the string back into an object.

References:
[# Recommended Drag Types](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API/Recommended_drag_types#dragging_custom_data)

[# JSON.stringify()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)

[# JSON.parse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)


#### In the Vue Dev Tools we can see setup (others)
![Vue Dev Tools](./screens/Schermata%202022-03-22%20alle%2018.24.08.png)

Here we find all the functions we have used in setup script, including the reactive function.

## Conclusions: 
### What we have learned?
Updating our knowledge may not be easy, but we have to fix in mind what is our goal.

In my opinion, we learn effectively in two situations: 
when we have a specific task to execute or when we create something for us. 
In either case, we have a project in mind, we study the specification step by step and have real problems to solve.

### Roadmap to study
1. Decide first what argument to study
2. Use Documentation, in this case, we have used Vuejs framework, then use other resources like MDN or w3school to learn more
3. Image a real-life situation end test your new knowledge
4. Read again documentation, explore and examine in-depth
5. Finally try to explain everything to someone.


