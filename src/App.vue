<template>
  <div id="app">
    <section class="main">
      <h1 class="main__title">CHARACTER Encyclopedia</h1>
    </section>
    <section class="search">
      <Autocomplete :items="persons"
        filterby="name"
        @change="onChange"
        title="Look for a Star Wars heroes"
        @selected="customerSelected"/>
      </section>
  </div>
</template>

<script>
import axios from 'axios'
import Autocomplete from './components/Autocomplete'

export default {
  name: 'App',
  data() {
        return {
          url: "https://swapi.dev/api/people/",
          persons: [],
          results: []
      }
   },
  created(){
      this.getPersons();
  },
  methods: {
    getPersons() {
      axios.get(this.url)
      .then(response =>  {
          console.log(response);
          this.persons = response.data.results;
      })
    },
    customerSelected(person) {
      console.log(`Customer Selected:\nid: ${person.id}\nname: ${person.name}`);
    },
    onChange(value) {
      // do something with the current value
    }
  },
  components: {
    Autocomplete
  }
}
</script>

<style lang="scss">

body {
  margin: 0;
  padding: 0;
}
#app {
  margin: 0;
  padding: 0;
}

.main {
  width: 100%;
  height: 480px;
  background-image: url("assets/images/main_bg.png");
  background-repeat: no-repeat;
  background-size: cover;

  &__title {
    font-family: 'Open Sans', sans-serif;
    font-style: normal;
    font-weight: bold;
    font-size: 24px;
    line-height: 28px;
    text-align: center;
    letter-spacing: 0.11em;
    text-transform: uppercase;
    color: #fff;
    margin: 0;
    padding-top: 200px;
    position: relative;

    &::before {
      content: "";
      position: absolute;
      background-image: url("assets/images/star_img.png");
      width: 406px;
      height: 96px;
      left: 0;
      right: 0;
      top: 90px;
      margin: auto;

      @media (max-width: 650px) {
        width: 80%;
        background-size: contain;
        background-repeat: no-repeat;
        top: 140px;
      }
    }

    &::after {
      content: "";
      position: absolute;
      background-image: url("assets/images/wars_img.png");
      width: 406px;
      height: 96px;
      top: 240px;
      left: 0;
      right: 0;
      margin: auto;

      @media (max-width: 650px) {
       width: 80%;
       background-size: contain;
       background-repeat: no-repeat;
       top: 265px;
      }
    }

    @media (max-width: 650px) {
        width: 80%;
        margin: 0 auto;
      }

  }
}

.search {
  background: #333333;
  padding-bottom: 100%;
  padding-top: 50px;
}
</style>
