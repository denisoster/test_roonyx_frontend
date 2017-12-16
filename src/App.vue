<template>
  <div id="demo">
    <form id="search">
      Total revenue {{total}}<br>
      From <datepicker v-model="from" name="from"></datepicker>
      To <datepicker v-model="to" name="to"></datepicker>
      <input name="query" type="button" value="Search" v-on:click="callData">
    </form>
    <table>
      <thead>
        <tr>
          <th v-for="key in gridColumns">
            {{ key }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="entry in gridData">
          <td v-for="key in gridColumns">
            {{entry[key]}}
          </td>
        </tr>
      </tbody>
    </table>
</div>
</template>

<script>
 import axios from "axios"
 import Datepicker from 'vuejs-datepicker';

export default {
  components: {
        Datepicker
  },
  props: {
    data: Array,
    columns: Array,
    filterKey: String
  },
  data:()=>{
    return{
      sortKey: '',
      searchQuery: '',
      from:new Date("2017-03-01"),
      to:new Date("2017-03-01"),
      total:0,
      gridColumns: ['title', 'revenue'],
      gridData: [
    
      ]
    }
  },
  computed: {},
  filters: {
  },
  methods: {
    callData: function(){
      var thet = this

      axios.get('http://localhost:3000/sales?from='+this.from.toISOString().substring(0, 10)+'&to='+this.to.toISOString().substring(0, 10)).then(
        function (res) {
          thet.gridData = res.data.good 
          thet.total = res.data.total_revenue
        }).catch(
        function(res){
          alert(res)
        }
      )
    }
  },
  created:function(){
    this.callData();
  }
}
</script>

<style>
body {
  font-family: Helvetica Neue, Arial, sans-serif;
  font-size: 14px;
  color: #444;
}

table {
  border: 2px solid #42b983;
  border-radius: 3px;
  background-color: #fff;
}

th {
  background-color: #42b983;
  color: rgba(255,255,255,0.66);
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

td {
  background-color: #f9f9f9;
}

th, td {
  min-width: 120px;
  padding: 10px 20px;
}

th.active {
  color: #fff;
}

th.active .arrow {
  opacity: 1;
}

.arrow {
  display: inline-block;
  vertical-align: middle;
  width: 0;
  height: 0;
  margin-left: 5px;
  opacity: 0.66;
}

.arrow.asc {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-bottom: 4px solid #fff;
}

.arrow.dsc {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-top: 4px solid #fff;
}

</style>
