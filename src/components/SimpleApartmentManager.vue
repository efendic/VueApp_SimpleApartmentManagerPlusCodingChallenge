<template>
  <div class="root">

    <div class="wrapper">
    <div id="div1" class="div1" v-if="currentApartment" style="float: left;">
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
            <td>
              <select v-model="currentApartment.state">
                <option value="active">active</option>
                <option value="assigned">assigned</option>
                <option value="assigned">inactive</option>
                <option value="assigned">reserved</option>
                <option value="vacant">vacant</option>
              </select>
            </td>
            <td> <input type="number" name="area" v-model="currentApartment.area" /> </td>
            <td> <input type="number" name="rooms" v-model="currentApartment.rooms" /> </td>
            <td>
              <select v-model="currentApartment.lift">
                <option value="true">true</option>
                <option value="false">false</option>
              </select>
            </td>
            <td> <input type="text" name="adress" v-model="currentApartment.building.adress" /> </td>
            <td> <input type="number" name="rentalgross" v-model="currentApartment.rentalgross" /> </td>
            
            <td> <button class="saveButton" type="button" @click.prevent.stop="saveApartment">Save</button> </td>
          </tr>
        </tbody>
      </table>
    </div>

    <div id="div2" class="div2">
      <button class="newApartment" @click.prevent.stop="newApartment()">New</button>
    </div>

    </div>

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
          <td> <span>{{ item.building ? item.building.adress : '' }}</span> </td>          
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
      currentApartment: null,
      config: {
        first_column: "title",
        second_column: "state",
        third_column: "area"
      }
    };
  },
  methods: {
    newApartment: function() {
      this.currentApartment = { index : this.apartments.length };
      this.currentApartment.building = { adress : ""};
    },
    editApartment: function(index) {
      this.currentApartment = Object.assign({}, this.apartments[index], { index: index });
    },
    deleteApartment: function(index) {
      this.apartments.splice(index, 1);
    },
    saveApartment: function() {
      var isUnique = true;
      this.apartments.forEach(element => {
        if (element.title == this.currentApartment.title) {
          isUnique = false;
        }
      });
      if (isUnique) {
        Vue.set(this.apartments, this.currentApartment.index, this.currentApartment);
        this.currentApartment = null;
      } else {
        alert('Apartment with the same title already exists!');
      }
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

.div1 {
  margin: 0px auto;
  margin-top: 10px;
  clear:both;
}

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

input, select {
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
