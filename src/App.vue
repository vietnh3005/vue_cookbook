<template>
  <div id="app">
    <h1>Random User</h1>
    <div class="person" v-for="person in persons" v-bind:key="person">
      <div class="left">
        <img :src="person.picture.large" />
      </div>
      <div class="right">
        <p>{{ person.name.first }} {{ person.name.last }}</p>
        <ul>
          <li><strong>Birthday:</strong> {{ person.dob }}</li>
          <li class="text-capitalize">
            <strong>Location:</strong> {{ person.location.city }},
            {{ person.location.state }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "app",
  data() {
    return {
      persons: []
    };
  },
  methods: {
    getInitialUsers() {
      for (var i = 0; i < 5; i++) {
        axios.get(`https://randomuser.me/api/`).then(response => {
          this.persons.push(response.data.results[0]);
        });
      }
    },
    scroll(person) {
      window.onscroll = () => {
        let bottomOfWindow =
          document.documentElement.scrollTop + window.innerHeight ===
          document.documentElement.offsetHeight;

        if (bottomOfWindow) {
          axios.get(`https://randomuser.me/api/`).then(response => {
            this.persons.push(response.data.results[0]);
          });
        }
      };
    }
  },
  beforeMount() {
    this.getInitialUsers();
  },
  mounted() {
    this.scroll(this.person);
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
