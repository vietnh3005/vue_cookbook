<template>
  <div id="app">
    <h2>Cats</h2>
    <div v-for="(cat, n) in cats" v-bind:key="(cat, n)">
      <p>
        <span class="cat">{{ cat }}</span>
        <button @click="removeCat(n)">Remove</button>
      </p>
    </div>

    <p>
      <input v-model="newCat" />
      <button @click="addCat">Add Cat</button>
    </p>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "app",
  data() {
    return {
      cats: [],
      newCat: null
    };
  },
  mounted() {
    if (localStorage.getItem("cats")) {
      try {
        this.cats = JSON.parse(localStorage.getItem("cats"));
      } catch (e) {
        localStorage.removeItem("cats");
      }
    }
  },
  methods: {
    addCat() {
      // ensure they actually typed something
      if (!this.newCat) {
        return;
      }

      this.cats.push(this.newCat);
      this.newCat = "";
      this.saveCats();
    },
    removeCat(x) {
      this.cats.splice(x, 1);
      this.saveCats();
    },
    saveCats() {
      const parsed = JSON.stringify(this.cats);
      localStorage.setItem("cats", parsed);
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
