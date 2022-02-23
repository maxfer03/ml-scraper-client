<template>
 <div>
   <button @click="checkLoading">?</button>
    <form v-on:submit="launchPython">
      <div class="queries" >
        <input v-model="currentQuery" type="text" placeholder="data"> <button v-on:click="addQuery">+</button>
      </div>
      <input v-model="category" type="text" placeholder="category">
      <input v-model="location" type="text" placeholder="location">
      <input type="submit" >
    </form>
    <div v-if="!loading">
      to search:
      <div v-for="el, idx in queries" :key="idx">
        {{el}}
      </div>
      <br/>
      at
      <div>https://{{category}}.mercadolibre.com.{{location}}/</div>
    </div>
    <div v-else>
      ...
    </div>
 </div>
 
</template>

<script>
//import axios from 'axios'
//import { saveAs } from 'file-saver';

export default {
  name: 'App',
  data(){
    return{
      loading: false,
      currentQuery: '',
      queries: [],
      category: '',
      location: ''
    }
  },
  methods: {
    async launchPython(event){
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
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;


  display: flex;
  flex-direction: column;
  align-items: center;

  form{
    width: 300px;
    display: flex;
    flex-direction: column;
  }

}
</style>
