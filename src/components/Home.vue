<template>
  <div>
    <h1>Fuck off generator</h1>
    <form @submit.prevent="getNewFoaas">
      <select v-model="state.options.choiceId">
        <option :value="i" v-for="(choice, i) in state.choicesSelect" :key="i">
          {{ choice.name }}
        </option>
      </select>
      <fieldset>
        <label>From:</label>
        <input
          type="text"
          v-model="state.options.from"
          placeholder="Your name"
        />
      </fieldset>

      <fieldset v-if="state.choicesSelect[state.options.choiceId].toPossible">
        <label>To:</label>
        <input
          type="text"
          v-model="state.options.to"
          placeholder="His/her name"
        />
      </fieldset>

      <button>Click !</button>
    </form>
    <div>
      <h2>{{ state.response.message }}</h2>
      <h3 v-if="state.response.subtitle">{{ state.response.subtitle }} -</h3>
    </div>
  </div>
</template>

<script>
import { reactive } from "vue";
import { optionsList } from "../assets/optionsList";

export default {
  name: "Home",
  setup() {
    const state = reactive({
      urlApi: "https://foaas.com/",
      choicesSelect: optionsList,
      options: {
        choiceId: 0,
        from: "",
        to: "",
        company: null,
        name: null,
      },
      response: {},
    });

    async function getNewFoaas() {
      const headers = {
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
      };


      try {
        const blob = await fetch(
          state.urlApi +
            state.choicesSelect[state.options.choiceId].request +
            "/" +
            state.options.to +
            "/" +
            state.options.from,
          headers
        );
        const datas = await blob.json();
        state.response = datas;
      } catch (err) {
        console.warn(err);
      }
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
