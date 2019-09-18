<template>
  <div class="root">
    <form v-if="currentApartment">
      <table>
        <thead>
          <tr>
          <th> Title </th>
          <th> <span>State</span> </th>
          <th> <span>Area</span> </th>
          <th> <span>Rooms</span> </th>
          <th> <span>Lift</span> </th>
          <th> <span>Adress</span> </th>
          <th> <span>Rentalgross</span> </th>
        </tr>
        </thead>
        <tbody>
          <tr>
            <td> <input type="text" name="title" v-model="currentApartment.title" /> </td>
            <td> <input type="text" name="state" v-model="currentApartment.state" /> </td>
            <td> <input type="text" name="area" v-model="currentApartment.area" /> </td>
            <td> <input type="text" name="rooms" v-model="currentApartment.rooms" /> </td>
            <td> <input type="text" name="lift" v-model="currentApartment.lift" /> </td>
            <td> <!-- <input type="text" name="adress" v-model="currentApartment.adress" /> --> </td> <!-- fix this -->
            <td> <input type="text" name="rentalgross" v-model="currentApartment.rentalgross" /> </td>
            
            <td> <button class="saveButton" type="button" @click.prevent.stop="saveApartment">Save</button> </td>
          </tr>
        </tbody>
      </table>
      

    </form>
    <button class="newApartment" @click.prevent.stop="newApartment()">New</button>
    <table>
      <thead>
        <tr>
          <th> Title </th>
          <th> <span>State</span> </th>
          <th> <span>Area</span> </th>
          <th> <span>Rooms</span> </th>
          <th> <span>Lift</span> </th>
          <th> <span>Adress</span> </th>
          <th> <span>Rentalgross</span> </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item,index) in apartments" v-bind:key="index">
          <td> <strong>{{ item.title }}</strong> </td>
          <td> <span>{{ item.state }}</span> </td>
          <td> <span>{{ item.area }}</span> </td>
          <td> <span>{{ item.rooms }}</span> </td>
          <td> <span>{{ item.lift }}</span> </td>
          <td> <span> Kanzleistrasse 126 </span> </td> <!-- fix this -->
          <td> <span>{{ item.rentalgross }}</span> </td>
          <td> 
            <button @click.prevent.stop="editApartment(index)">Edit</button>
            <button class="deleteButton" @click.prevent.stop="deleteApartment(index)">Delete</button> 
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
import Vue from "vue";

var model = {
  name: "SimpleApartmentManager",
  data() {
    return {
      apartments: [],
      currentApartment: null
    };
  },
  methods: {
    newApartment: function() {
      this.currentApartment = { index: this.apartments.length };
    },
    editApartment: function(index) {
      this.currentApartment = Object.assign({}, this.apartments[index], { index: index });
    },
    deleteApartment: function(index) {
      this.apartments.splice(index, 1);
    },
    saveApartment: function() {
      Vue.set(this.apartments, this.currentApartment.index, this.currentApartment);
      this.currentApartment = null;
    }
  },
  mounted() {
    axios.get("https://erstvermietung.emonitor.ch/api/objects/").then(result => {
      this.apartments = result.data;
      // console.log(result.data);
    });
  }
};

export default model;
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

h3 {
  margin: 40px 0 0;
}

table {
  text-align: left;
  border-collapse: collapse;
  width: 100%;
}

th, td {
  padding: 15px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

input {
  height: 48px;
  font-size: 16px;
  border: 1px solid #4CAF50;
}

button {
  background-color: #008CBA;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}

.newApartment, .saveButton {
  background-color: #4CAF50;
}

.newApartment {
  float: left;
}

.deleteButton {
  background-color: #f44336;
  margin-left: 5px;
}

a {
  color: #42b983;
}

</style>
