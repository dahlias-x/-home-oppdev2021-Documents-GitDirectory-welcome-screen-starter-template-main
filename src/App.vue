<template>
  <div id="app">

  <div>
    <h1 class="site-title" >{{ title }}..</h1>
  </div>
  <span class="site-description" >{{ currentDate }}</span>
  <ul class="entry-list">
    <li 
      v-for="entry in filteredEntries"
      :key="entry.id"    
      class="entry-item"
      >
    <span class="entry-dayTime">{{ entry [0]}} Uhr,{{ entry[1].replaceAll("/",".")}} </span>
      <br>
      <h3 class="entry-title">{{ entry[2] }}</h3>
      <span class="entry-description">{{ entry[3] }}</span>
      <br>
    </li>
    <!-- <li class="entry-item">
      <span class="entry-dayTime">{{ currentDate }}</span>
      <br>
      <h3 class="entry-title">Tugce Nur returns to office</h3>
      <span class="entry-description"></span>
      <br>
    </li>
    <li class="entry-item">
      <span class="entry-dayTime">{{ currentDate }}</span>
      <br>
      <h3 class="entry-title">Firmenbesuch @ panter</h3>
      <span class="entry-description"></span>
      <br>
    </li> -->
  </ul>
  {{updateCurrentDate()}}
  <footer class="footer">
    <img class="img" src="./assets/STZH_SEB_Logo.png"  alt="Stadt Zürich Soziale Betriebe und Einrichtungen Logo">
    <img class="img" src="./assets/Opportunity.png" alt="Opportunutiy Logo">
    <img class="img" src="./assets/SAG_Logo_De.png"  alt="Stiftung Arbeitsgestaltung Logo">
  </footer>

  </div>
</template>

<script>
import axios from "axios";

export default {
  name: 'App',
  data() {
    return {
        title: "Welcome to Opportunity",
        // date:  new Date().toISOString().slice(8,10)+"."+new Date().toISOString().slice(5,7)+"."+new Date().toISOString().slice(0,4)
        currentDate:"",
        gesheet_url:"https://sheets.googleapis.com/v4/spreadsheets/1qLZJwuNv3QmwGhSj1wZZbuXNOkDKN-Ha7fo0Ca_uVVU/values:batchGet?ranges=A1%3AE100&valueRenderOption=FORMATTED_VALUE&key=AIzaSyATblrWN5BdW4mLMV4uY3glRZQuwX-rMqU",
        entries:[],
      };
    
    },
    computed:{
        filteredEntries() {
            return[...this.entries].slice(1);
        }
    },

    methods: {
      getData(){
        axios.get(this.gesheet_url).then((response)=> {
          this.entries = response.data.valueRanges[0].values;
    
        }); 
      },
      updateCurrentDate(){
        let today = new Date();
        const date = `${today.getDate()}.${today.getMonth()+1}.${today.getFullYear()}`;
        this.currentDate = date;
      },
      refreshData(){
        this.getData();
        this.updateCurrentDate();
      },

    },
    mounted(){
      this.refreshData(); //get first initial data and then wait for the next update
      setInterval(() => {
        this.refreshData();
      },1800000) // wait 30 min for next update
    }
}
</script>

<style>
/* css styles go here */


@import url('https://fonts.googleapis.com/css2?family=Inter:wght@500;700;900&display=swap');
#app {
  font-family: Helvetica, Inter, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  
  color: #323d4a;
  margin-top: 60px;
}

body {
 background:#e8eff3;
 height: 1920px;
}
* {
  margin: 0;
  padding: 0;
}

.entry-item{
  padding: 35px 40px;
  margin: 40px 40px;
  background:#0f05a0;
  font-size:28px;
  line-height:1.3;
  list-style:none;

}
.img{

  height:50px;
  
}
.footer{
  display:flex;
  box-sizing: border-box;
  background:#FFFFFF;
  padding:40px;
  position:fixed;
  bottom:0;
  left:0;
  width:100%;
  justify-content: space-between;
  
}
.entry-description{
  font-weight:500;
  color:#ffbfab
  
}

.entry-dayTime{
  font-weight:900;
  color:#eb5e00;
}
.site-title{
Margin: 80px 40px 20px;
font-size: 62px;
font-weight:900;
font-family: Inter; 
font-style: bolder;

}
.site-description{
  font-size:62px;
  font-weight:500;
  margin:40px;
  color:#9aa7b1;
}
.entry-list{
  padding-left: 0;
}
.entry-title{
  font-size:inherit;
  font-weight:900;
  color:#ffbfab;
  margin:0;
}
</style>
