<template>
 <div>
    <va-form class="search-form" >
      <div class="queries" >
        <va-input class="mb-4 mr-4" label="Search for"  v-model="currentQuery" type="text" placeholder="data"></va-input> <va-button icon="add"  v-on:click="addQuery"></va-button>
      </div>
      <va-select class="mb-4" label="Category" v-model="category" :options="categories" ></va-select>
      <va-select class="mb-4" label="Country" v-model="location" :options="locations"></va-select>
      <va-card class="bottom-box mb-4"  v-if="!loading">
        <span>To search:</span>
        <span v-if="queries.length < 1">
          Nothing here yet.
        </span>
        <div v-else class="queries-container">
          <div class="query" v-for="el, idx in queries" :key="idx">
            <span>{{el}}</span>
            <va-button icon="close"  v-on:click="rmQuery(idx)"></va-button>
          </div>
        </div>
        <span>at</span>
        <div>{{generateUrl}}</div>
      </va-card>
      <va-card  class="bottom-box mt-4" v-else>
        ...
      </va-card>
      <va-button type="submit" v-on:click="launchPython" >Search</va-button>
    </va-form>
 </div>
 
</template>

<script>
//import axios from 'axios'
//import { saveAs } from 'file-saver';
export default {
  name: 'App',
  components: {
  },
  data(){
    return{
      loading: false,
      currentQuery: '',
      queries: [],
      categories: ['Ropa','Listado','Hogar','test4','test5'],
      category: 'Ropa',
      locations: ['ar', 'co', 'mx'],
      // locations: [
      //   {
      //   domain: 'ar',
      //   country: "Argentina"
      //   },
      //   {
      //   domain: 'co',
      //   country: "Colombia"
      //   },
      //   {
      //   domain: 'uy',
      //   country: "Uruguay"
      //   },
      // ],
      location: 'ar'
    }
  },
  computed:{
    generateUrl(){
      return `https://${this.category.toLowerCase()}.mercadolibre.com.${this.location}`
    }
  },
  methods: {
    async launchPython(event){
      if (this.queries.length < 1) {
        alert("You haven't specified what to look for!")
        return 0
      }

      this.loading = true
      console.log(this.loading)
      event.preventDefault()
     
      let data = encodeURIComponent(JSON.stringify(this.queries))
      let cat = this.category
      let loc = this.location
  
      try{
        let url =  `http://localhost:5000/scrape_data?data=${data}&cat=${cat}&loc=${loc}`

        var link = document.createElement('a');
        link.href = url
        link.click()
        this.queries = []
        this.currentQuery = ''
      }
      catch(e){
        console.log('oops!', e)
      }


      this.loading = false
      console.log(this.loading)

    },
    addQuery(event){
      event.preventDefault()
      this.queries.push(this.currentQuery)
      this.currentQuery = ''
    },
    rmQuery(i){
      console.log('sup!', i)
      this.queries.splice(i, 1)
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Source Sans Pro';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;


  display: flex;
  flex-direction: column;
  align-items: center;

  .search-form{
    .queries{
      display: flex;
      justify-content: space-between;
    }

    
  }

  .bottom-box{
    
    padding: 20px;
    .va-card__inner{
      //color: white;
      display: flex;
      flex-direction: column;
      span{
      width: initial;
      margin: 0px 0px 10px 0px;
      }

      .queries-container{
        display: flex;
        flex-direction: column;
        .query{
          margin: 5px 0px;
          display: flex;
          align-items: center;
          justify-content: space-between;
        }
      }
    }
    
  }

}
</style>
