<template>
  <div>
    <h1>Générateur de fuck off</h1>
    <form @submit.prevent="getNewFoaas">
      <button>Cliquez !</button>
    </form>
    <div>
      <h2>{{ state.response.message }}</h2>
      <h3>{{ state.response.subtitle }}</h3>
    </div>
  </div>
</template>

<script>
import { reactive } from "vue";

export default {
  name: "Home",
  setup() {
    const state = reactive({
      urlApi: "https://foaas.com/awesome/",
      choicesSelect: [],
      options: {
        from: "toto",
        company: null,
        name: null,
      },
      response: {},
    });

    function getNewFoaas() {
      const headers = {
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json'
        }
      }

      return fetch(state.urlApi + state.options.from, headers)
        .then((blob) => blob.json())
        .then((datas) => {
          console.log(datas);
          state.response = datas
/* 
          charactersInfos.push(...datas.results);
          infosSupp.push(...datas.info); */
        })
        .catch((err) => {
          console.warn(err);
        });
    }

    return {
      state,
      getNewFoaas,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
