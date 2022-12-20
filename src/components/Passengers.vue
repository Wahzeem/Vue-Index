<template>

  <div>

  <h3>Passengers</h3>

<!--Pagination bar-->
  <b-pagination
      align="center"
      v-model="currentPage"
      :total-rows="rows"
      :per-page="perPage"
      aria-controls="results-table"
    ></b-pagination>

<!--Items per page menu-->
<div class="perPage">
<label>Results per page:</label>
<select v-model="perPage">
  <option v-for="pages in pageLimit " :key="pages">
    {{pages}}
  </option>
  <option :value="passengers.length">
    All
  </option>
</select>
</div>

<!--Results table-->
  <b-table
      id="results-table"
      :items="filtered"
      :per-page="perPage"
      :current-page="currentPage"
      small>

      <template slot="top-row" slot-scope="{fields}">
        <td v-for="field in fields" :key="field.key">
          <input class="col-sm" v-model="filters[field.key]" :placeholder="field.label">
        </td>
      </template>

    </b-table>

  </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";
Vue.use(VueAxios, axios);
export default {
  name: 'Passengers',
  mounted(){
    this.getData();
  },
  data(){
    return{
      currentPage: 1, //Starting page
      perPage: 20, //Default of 20 passengers per page
      pageLimit: 50, //Limitation to results per page
      apiURL: "http://coding-task.strakertranslations.com/passengers",//API URL
      passengers: [], //empty Array or api data
      filters: {  //When the filter input is typed in, filters will equal to the field name of that input. eg...Name: "input..."
        // Name: "",
        // Age: "",
        // Ticket: "",
        // Embarked: "",
        // class: ""
        //TODO Wanted to narrow down on filtering capabilities to these specific groups
      }
    }
  },

  methods: {
    getData(){
      axios.get(this.apiURL) //fetch data from this URL
           .then((response) =>{
             this.passengers = response.data; //make passengers array list equal to api response data
             // console.log(response);
             console.log(this.filters);
           })
    }
  },

  computed:{ //Computed Properties
    rows(){ //Number of rows
      return this.passengers.length;
    },
    filtered(){ //search filter functionality
      const filtered = this.passengers.filter(item => { //filter passengers data into new array
        return Object.keys(this.filters).every(key =>
          String(item[key]).includes(this.filters[key]));

      })
      return filtered.length > 0 ? filtered :[{ //if the filtered data is greater than 0 then use filtered, else empty the input values
        id:"",
        Survived:"",
        class: "",
        Name: "",
        Sex: "",
        Age: "",
        Siblings:"",
        Parch: "",
        Ticket: "",
        Fare: "",
        Cabin: "",
        Embarked: "",

      }]

    }
  },
}
</script>

<!-- CSS confined to this component -->
<style scoped>

h3 {
  margin: 30px;
}
li {
  display: inline-block;
  margin: 0 10px;
}

p{
  margin: 20px;
}

select{
  margin-left: 10px;
}

.perPage {
  float:left;
  margin: 20px;
}

.title{
  font-weight: bold;
}

.perPage {
  float:left;
  margin: 10px;
}

</style>
