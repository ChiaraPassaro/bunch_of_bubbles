<script setup>
  // This starter template is using Vue 3 <script setup> SFCs

  import {
    reactive
  } from 'vue';

  //we use the reactive function to declare a state
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

  //these are our methods
  function toBasket(index) {
    //we remove the element from stack
    const [bubble] = state.bubbles.splice(index, 1);

    //we add bubble to relative basket
    state.baskets.forEach(element => {
      if (element.name == bubble.name) {
        element.bubbles.push(bubble);
      }
    });
  }

  function toTopStack(indexBasket, indexBubble) {
    //we remove the element from basket
    const [bubble] = state.baskets[indexBasket].bubbles.splice(indexBubble, 1);
    //we add element on top of bubbles
    state.bubbles.unshift(bubble);
  }

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
</script>

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
          @click="toBasket(indexBubble)"
          @dragover.prevent
          @dragenter.prevent
          @drop="changeColor($event, indexBubble)"
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
              @click="toTopStack(indexBasket,indexBubble)"
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
        draggable="true"
        @dragstart="setColor($event, color)"
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