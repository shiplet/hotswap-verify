<template>
  <h1>
    Welcome to the
    <span class="highlight">Hotswap Keyboard Sanity Checker</span>
  </h1>
  <p>
    Just type each key on your keyboard, without any modifiers (ctrl, shift,
    etc..) and this will tick off each one you type.
  </p>
  <p>
    Note that because some keys are duplicates, you may find empty boxes even
    though it worked. Just tap the blank key a few times and it should toggle
    the dupe. If not, you've got a key that needs re-seating!
  </p>
  <p>
    It's currently only set up for a US-style tenkeyless, and my personal layout
    of the ZSA Moonlander.
  </p>
  <p>
    If you'd like to add a new layout, open a PR at my github repo:
    <a href="https://github.com/shiplet/hotswap-verify" target="_blank"
      >hotswap-verify</a
    >.
  </p>
  <div class="selectContainer">
    <select
      class="styledSelect"
      name="layout"
      id="layout"
      v-model="selectedLayout"
    >
      <option value="" hidden>Select Layout</option>
      <option
        v-for="layout in availableLayouts"
        :value="layout"
        :key="`layout-${layout}`"
      >
        {{ layout }}
      </option>
    </select>
  </div>
  <div class="keyContainer">
    <div
      class="keyRow"
      v-for="row in Object.keys(layouts[selectedLayout].keys)"
      :key="{ row }"
    >
      <Key
        v-for="code in layouts[selectedLayout].keys[row]"
        :key="code"
        :code="code"
        :pressed="pressedKeys.has(code)"
      />
    </div>
  </div>
</template>

<script>
import Key from "./components/Key.vue";
import { tenkeyless, moonlander } from "./layouts";

export default {
  name: "App",
  components: { Key },
  data() {
    return {
      pressedKeys: new Set(),
      selectedLayout: "tenkeyless",
      availableLayouts: ["tenkeyless", "moonlander"],
      layouts: {
        tenkeyless,
        moonlander,
      },
    };
  },
  mounted() {
    document.addEventListener("keydown", (e) => {
      e.preventDefault();
      if (this.pressedKeys.has(e.code)) {
        this.pressedKeys.delete(e.code);
      } else {
        this.pressedKeys.add(e.code);
      }
    });
  },
};
</script>

<style>
:root {
  --dark0_hard: #1d2021;
  --dark0: #282828;
  --dark0_soft: #32302f;
  --dark1: #3c3836;
  --dark2: #504945;
  --dark3: #665c54;
  --dark4: #7c6f64;
  --dark4_256: #7c6f64;
  --gray_245: #928374;
  --gray_244: #928374;
  --light0_hard: #f9f5d7;
  --light0: #fbf1c7;
  --light0_soft: #f2e5bc;
  --light1: #ebdbb2;
  --light2: #d5c4a1;
  --light3: #bdae93;
  --light4: #a89984;
  --light4_256: #a89984;
  --bright_red: #fb4934;
  --bright_green: #b8bb26;
  --bright_yellow: #fabd2f;
  --bright_blue: #83a598;
  --bright_purple: #d3869b;
  --bright_aqua: #8ec07c;
  --bright_orange: #fe8019;
  --neutral_red: #cc241d;
  --neutral_green: #98971a;
  --neutral_yellow: #d79921;
  --neutral_blue: #458588;
  --neutral_purple: #b16286;
  --neutral_aqua: #689d6a;
  --neutral_orange: #d65d0e;
  --faded_red: #9d0006;
  --faded_green: #79740e;
  --faded_yellow: #b57614;
  --faded_blue: #076678;
  --faded_purple: #8f3f71;
  --faded_aqua: #427b58;
  --faded_orange: #af3a03;
}

body {
  background-color: var(--dark0_hard);
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: var(--light0_hard);
}

a {
  color: var(--bright_aqua);
}

p,
div,
li,
span,
h1,
h2,
h3,
h4,
h5,
h6 {
  color: var(--light0_hard);
}

.highlight {
  color: var(--bright_green);
}

.selectContainer {
  margin: 50px auto;
  width: 20vw;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}

.selectContainer button {
  border: 1px solid var(--faded_aqua);
  background-color: var(--dark0_soft);
  color: var(--bright_aqua);
  padding: 8px 16px;
  border-radius: 5px;
  font-weight: bold;
  user-select: none;
}

.selectContainer button.active {
  background-color: var(--faded_aqua);
  color: var(--dark0_hard);
}

.styledSelect {
  padding: 6px 12px;
}

.keyRow {
  width: 80%;
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  margin-bottom: 15px;
}

.keyContainer {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-evenly;
}
</style>
