<template>
  <div id="app">
    <div class="fancySelect up" :class="choice1.isActive && 'active'">
      <div class="selectedOption" @click="choice1.isActive = !choice1.isActive">
        <span>
          {{ choice1.value }}
        </span>
      </div>
      <div class="dropdown">
        <ul class="dropdown_list">
          <li
            v-for="(el, idx) in choices"
            :key="`selectOne_${idx}`"
            :class="el.id === choice1.value && 'selected'"
          >
            <span @click="changeChoice(choice1, el.id)">
              <i class="icon-envelope icon-large"></i>
              {{ el.id }}
            </span>
          </li>
        </ul>
      </div>
    </div>
    <div class="fancySelect" :class="choice2.isActive && 'active'">
      <div class="selectedOption" @click="choice2.isActive = !choice2.isActive">
        <span>
          {{ choice2.value }}
        </span>
      </div>
      <div class="dropdown">
        <ul class="dropdown_list">
          <li
            v-for="(el, idx) in choices"
            :key="`selectOne_${idx}`"
            :class="el === choice2.value && 'selected'"
          >
            <span @click="changeChoice(choice2, el)">
              <i class="icon-envelope icon-large"></i>
              {{ el }}
            </span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      apiUrl: "https://free.currconv.com/api/v7/",
      apiKey: "bba7986b5cc69bcbd370",
      choices: {},
      choice1: {
        isActive: false,
        value: "Select Option",
      },
      choice2: { isActive: false, value: "Select Option" },
    };
  },
  methods: {
    showDrop(el) {
      el.target.classList.toggle("active");
    },
    changeChoice(target, value) {
      target.value = value;
      target.isActive = !target.isActive;
    },
  },

  mounted() {
    let url = `${this.apiUrl}currencies?apiKey=${this.apiKey}`;

    fetch(url)
      .then((res) => {
        return res.json();
      })
      .then((json) => {
        this.choices = json.results;
      });
  },
};

</script>

<style>
body {
  align-items: center;
  display: flex;
  justify-content: center;
  height: 100vh;
}

#app {
  font-family: "Montserrat", sans-serif;
}

/* select */
.fancySelect {
  /* Size and position */
  position: relative;
  width: 200px;
  margin: 0 auto;
  margin-bottom: 30px;

  /* Styles */
  background: #fff;
  border-radius: 5px;
  border: 1px solid rgba(0, 0, 0, 0.15);
  box-shadow: 0 1px 1px rgba(50, 50, 50, 0.1);
  cursor: pointer;
  outline: none;

  /* Font settings */
  font-weight: bold;
  color: #de5bbc;
}

.fancySelect.up {
  z-index: 6;
}

.selectedOption {
  display: block;
  padding: 15px 10px;
  position: relative;
  font-weight: 400;
}

.selectedOption:after {
  content: "";
  width: 0;
  height: 0;
  position: absolute;
  right: 15px;
  top: 50%;
  margin-top: -3px;
  border-width: 6px 6px 0 6px;
  border-style: solid;
  border-color: #de5bbc transparent;
}

/* Dropdown List */
.dropdown {
  /* Size & position */
  position: absolute;
  top: 140%;
  left: 0;
  right: 0;
  z-index: 5;

  /* Styles */
  background: white;
  border-radius: inherit;
  border: 1px solid rgba(0, 0, 0, 0.17);
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
  font-weight: normal;
  transition: all 0.5s ease-in;
  list-style: none;
  padding: 5px;

  /* Scrollbar */

  scrollbar-width: thin; /* "auto" or "thin" */
  scrollbar-color: blue orange; /* scroll thumb and track */

  /* Hiding */
  opacity: 0;
  pointer-events: none;
}

.dropdown:after {
  content: "";
  width: 0;
  height: 0;
  position: absolute;
  bottom: 100%;
  right: 15px;
  border-width: 0 6px 6px 6px;
  border-style: solid;
  border-color: #fff transparent;
}

.dropdown:before {
  content: "";
  width: 0;
  height: 0;
  position: absolute;
  bottom: 100%;
  right: 13px;
  border-width: 0 8px 8px 8px;
  border-style: solid;
  border-color: rgba(0, 0, 0, 0.1) transparent;
}

.dropdown_list {
  overflow-y: auto;
  max-height: 240px;
}

.dropdown_list::-webkit-scrollbar {
  width: 4px; /* width of the entire scrollbar */
}

.dropdown_list::-webkit-scrollbar-track {
  background: #f5f5f5; /* color of the tracking area */
}

.dropdown_list::-webkit-scrollbar-thumb {
  background-color: #de5bbc; /* color of the scroll thumb */
  border-radius: 20px; /* roundness of the scroll thumb */
}

.dropdown_list li.selected span {
  background: #f3f8f8;
}

.dropdown_list li {
  margin: 0 5px;
}

.dropdown_list li span {
  display: block;
  padding: 12px;
  text-decoration: none;
  color: #8aa8bd;
  border-top: 1px solid #e6e8ea;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 1);
  transition: all 0.3s ease-out;
}

.dropdown_list li:first-child span {
  border: 0;
}

.dropdown_list li i {
  float: right;
  color: inherit;
}

.dropdown_list li:first-of-type a {
  border-radius: 7px 7px 0 0;
}

.dropdown_list li:last-of-type a {
  border-radius: 0 0 7px 7px;
  border: none;
}

/* Hover State */
.dropdown_list li:hover span {
  background: #f3f8f8;
}

/* Select Active State */
.fancySelect.active .dropdown {
  opacity: 1;
  pointer-events: auto;
}
</style>
<!-- Selected Dropdown based on https://tympanus.net/codrops/2012/10/04/custom-drop-down-list-styling/ -->
