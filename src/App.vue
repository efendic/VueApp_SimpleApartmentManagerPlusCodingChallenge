<template>
  <div id="app">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
    <div class="tabs">
      <div class="tab-navigation">
        <button @click="changeTab(0)" :class="{ focused: selectedTab === 0 }">Simple Apartment Manager</button>
        <button @click="changeTab(1)" :class="{ focused: selectedTab === 1 }" style="margin-left: 5px;">Coding Challenge</button>
      </div>
      <br>
      <br>
      <br>
      <div v-if="selectedTab === 0">
        <SimpleApartmentManager />
      </div>
      <div v-if="selectedTab === 1">
        <textarea
          @keyup="findMostOccuring()"
          name="codingChallenge"
          id="cct"
          cols="30"
          rows="10"
          placeholder="Type text here..."
          v-model="characters"
        ></textarea>
        <br>
        <label v-if="maxKey"> Most occuring character: <strong>{{maxKey}}</strong>. Occuring <strong>{{expCounts}}</strong> times. </label>
      </div>
    </div>
  </div>
</template>

<script>
import SimpleApartmentManager from "./components/SimpleApartmentManager.vue";
export default {
  name: "app",
  components: {
    SimpleApartmentManager
  },
  data() {
    return {
      selectedTab: 0,
      characters: null,
      maxKey: null,
      expCounts: null
    };
  },
  methods: {
    changeTab: function(index) {
      this.selectedTab = index;
    },
    findMostOccuring: function() {
      var exp = this.characters;
      var expCounts = {};
      var maxKey = "";
      for (var i = 0; i < exp.length; i++) {
        var key = exp[i];
        if (!expCounts[key]) {
          expCounts[key] = 0;
        }
        expCounts[key]++;
        if (maxKey == "" || expCounts[key] > expCounts[maxKey]) {
          maxKey = key;
        }
      }
      // console.log(maxKey + ":" + expCounts[maxKey]);
      this.maxKey = maxKey;
      this.expCounts = expCounts[maxKey]  ;
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

.tabs {
  border: 1px solid #ddd;
  padding: 15px;
}

.tab-navigation {
  float: left;
}

textarea{
  width:600px;
  height: 300px;
  font-size: 16px;
}

.tab-navigation > button {
  background-color: lightgrey;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}

.tab-navigation > button:focus,
.tab-navigation > button.focused {
  background-color: #31B0D5;
  color: white;
}
</style>
