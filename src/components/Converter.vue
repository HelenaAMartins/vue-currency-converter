
<template>
  <div class="converter">
    <!-- ERROR MESSAGE -->
    <div class="errorMsg" :class="error ? 'show' : ''">
      Preencha todos os campos
    </div>
    <!-- CURRENCY A INPUT -->
    <label for="inputCur">Type how much you'd like to convert</label>
    <input
      class="inputCur"
      id="inputCur"
      @focus="setError(false)"
      @change="currencyA.id && currencyB.id && currencyA.value && convert()"
      type="text"
      v-model="currencyA.value"
    />
    <!-- SELECT CURRENCY A -->
    <div class="fancySelect">
      <select @change="changeCurA($event)">
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
    <!-- SELECT CURRENCY B -->
    <div class="fancySelect">
      <select @change="changeCurB($event)">
        <option value="" disabled selected hidden>Select your currency</option>
        <option
          v-for="(currency, index) in currencies"
          :key="`currencyB_${index}`"
          :value="currency.id"
        >
          {{ `${currency.id} | ${currency.currencyName} ` }}
        </option>
      </select>
    </div>

    <!-- CONVERT BUTTON -->
    <input class="btn" type="button" value="Convert" v-on:click="convert" />

    <!-- Convertion text -->
    <h2 v-if="currencyA.value && currencyB.value">
      {{
        `${currencyA.value}${currencyA.id} = ${currencyB.value}${currencyB.id}`
      }}
    </h2>
  </div>
</template>

<script>
export default {
  name: "Converter",

  data() {
    return {
      apiUrl: "https://free.currconv.com/api/v7/",
      apiKey: "bba7986b5cc69bcbd370",
      currencyA: {
        id: "",
        value: "",
        symbol: "",
      },
      currencyB: {
        id: "",
        value: "",
        symbol: "",
      },
      currencies: {},
      error: false,
    };
  },
  methods: {
    /* Calculate the convertion between two currencies. */
    convert() {
      let from_to = this.currencyA.id + "_" + this.currencyB.id;
      let url = `${this.apiUrl}convert?q=${from_to}&compact=ultra&apiKey=${this.apiKey}`;

      if (this.currencyA.id && this.currencyA.value && this.currencyB.id) {
        fetch(url)
          .then((res) => {
            return res.json();
          })
          .then((json) => {
            let rate = json[from_to];
            this.currencyB.value = (
              rate * parseFloat(this.currencyA.value)
            ).toFixed(2);
          });
      } else {
        this.setError(true);
      }
    },

    /**
     * Handle the error state. */
    setError(e) {
      this.error = e;
      setTimeout(() => (this.error = false), 1500);
    },

    /**
     * CHANGE CURRENCY A VALUE */
    changeCurA(el) {
      this.currencyA.id = el.target.value;
      this.setError(false);
    },

    /**
     * CHANGE CURRENCY B VALUE. */
    changeCurB(el) {
      this.currencyB.id = el.target.value;
      this.setError(false);
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
      });
  },
};
</script>

<style scoped>
.converter {
  padding: 30px;
  max-width: 300px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  overflow: hidden;
  width: 90%;
  margin: 0 auto;
  background: #fff;
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 280px;
  position: relative;
}

.errorMsg {
  background: red;
  opacity: 0;
  visibility: hidden;
  transition: all 0.5s;
  transform: translateY(-50px);
  position: absolute;
  width: 100%;
  top: 0;
  padding: 20px;
  color: #fff;
  font-size: 12px;
  text-transform: uppercase;
  text-align: center;
}

.errorMsg.show {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}

label {
  text-align: left;
  width: 100%;
  color: rgb(77, 75, 72);
  font-size: 13px;
}
.inputCur {
  border: 0;
  border-bottom: 2px solid #e6306d;
  outline: none;
  transition: 0.2s ease-in-out;
  box-sizing: border-box;
  width: 100%;
  margin-bottom: 15px;
  padding: 10px 0;
  font-size: 30px;
  color: #a52b66;
}
/* Select */
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

/* button */
.btn {
  display: block;
  cursor: pointer;
  outline: none;
  border: none;
  background-color: #e6306d;
  width: 130px;
  padding: 10px 30px;
  border-radius: 25px;
  font-size: 13px;
  text-transform: uppercase;
  color: #fff;
  transition: all 0.5s;
}

.btn:hover {
  background: #a52b66;
}

h2 {
  color: #7a1646;
  font-weight: 300;
}
</style>