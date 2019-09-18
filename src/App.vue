<template>
  <div id="app">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
    <div class="tabs">
      <div class="tab-navigation">
        <button @click="changeTab(0)" :class="{ focused: selectedTab === 0 }">SimpleApartmentManager</button>
        <button @click="changeTab(1)" :class="{ focused: selectedTab === 1 }">coding</button>
      </div>
      <div v-if="selectedTab === 0">
        <SimpleApartmentManager />
      </div>
      <div v-if="selectedTab === 1">
        <textarea
          @keyup="keyupFun()"
          name="codingChallenge"
          id="cct"
          cols="30"
          rows="10"
          v-model="characters"
        ></textarea>
        <br>
        <label v-if="maxKey"> Most occuring character: '{{maxKey}}'. Occuring {{expCounts}} times. </label>
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
    keyupFun: function() {
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
  border: 1px solid gray;
  padding: 10px;
}

.tab-navigation > button {
  display: inline-block;
  margin: 3px;
  background-color: #fff;
  outline: none;
  border: 1px solid black;
}
.tab-navigation > button:focus,
.tab-navigation > button.focused {
  background-color: gray;
  color: white;
}
</style>
