<template>
  <div class="fancySelect">
    <select @change="execute($event)">
      <option value="" disabled selected hidden>Select your currency</option>
      <option
        v-for="(currency, index) in currencies"
        :key="`currencyA_${index}`"
        :value="currency.id"
      >
        {{ `${currency.id} | ${currency.currencyName} ` }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  name: "Select",

  data() {
    return {
      apiUrl: "https://free.currconv.com/api/v7/",
      apiKey: "bba7986b5cc69bcbd370",
      currencies: {},
    };
  },
  props: {
    callback: {
      type: Function,
    },
  },
  methods: {
    execute(e) {
      // ... do something here
      console.log("ei eu abestado")

      if (this.callback) {
        this.callback(e.target.value);
      }
    },
  },

  mounted() {
    let url = `${this.apiUrl}currencies?apiKey=${this.apiKey}`;

    // CREATION OF THE LIST OF CURRENCIES
    fetch(url)
      .then((res) => {
        return res.json();
      })
      .then((json) => {
        this.currencies = json.results;
        console.log(json.results);
      });
  },
};
</script>

<style scoped>
.fancySelect {
  width: 100%;
  position: relative;
  cursor: pointer;
  display: block;
  font-size: 1em;
  background: #f5f5f5;
  border-radius: 4px;
  padding: 1em;
  margin-right: -1em;
  border: 1px solid #e6306d;
  box-sizing: border-box;
  margin: 0 0 15px;
}
.fancySelect:after {
  box-sizing: border-box;
  position: absolute;
  text-align: center;
  right: -1px;
  top: 0;
  height: 100%;
  overflow: hidden;
  padding-top: 0.25em;
  width: 3.51562em;
  display: block;
  content: "\25BC";
  background: #e6306d;
  color: #fff;
  font-size: 0.75em;
  border-radius: 0 4px 4px 0;
  box-sizing: border-box;
  padding-top: 8px;
}
.fancySelect:focus:after {
  background: #f5f5f5;
  box-shadow: 0 0 4px #00c;
}
.fancySelect select {
  width: 100%;
  height: 100%;
  font-size: 0.8em;
  position: absolute;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  padding: 0.1em 0em 0em;
  background: transparent;
  outline: none !important;
  border: 0 none;
  z-index: 10;
  left: 0px;
  padding-left: 1em;
  top: 0;
  border-radius: 4px;
  overflow: visible;
}
.fancySelect select:focus {
  box-shadow: 0 0 6px #2f8aac;
  outline: none;
}
.fancySelect select::-ms-expand {
  display: none;
}
</style>

