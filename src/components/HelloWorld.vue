<template>
  <div class="text-center">
    <input type="text" v-model="search">
       <div>
           <table>
            <thead>
               <th>
                <td>id</td>
                 <td>name</td>
               </th>
              </thead>
              <tbody>
                 <tr v-for="user in mysearch" :key="user.id">
                    <td>{{user.login}}</td>
                 </tr>
              </tbody>
           </table>
       </div>
  
  </div>
</template>

<script>
import Axios from 'axios';
import "@/plugins/axios"
export default {
  name: 'HelloWorld',
  created(){
    this.fetchAllUsers();
  },
  methods:{
    async  fetchAllUsers() {
      let page = 1;
      const perPage = 100;
      const delay = (ms) => new Promise((resolve) => setTimeout(resolve, ms));
      const requestDelay = 500;
  try {
    while (true) {
      const response = await axios.get('https://api.github.com/search/users', {
        params: {
          //q:"TalelMejri"
          q: 'location:Tunisia',
          per_page: perPage,
          sort:"commits",
          //sort:"contributions",
          page,
        },
        headers: {
          Accept: 'application/vnd.github.v3+json',
          Authorization: 'Bearer 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',
        },
      });
      await delay(requestDelay); 
      this.AllUser = this.AllUser.concat(response.data.items).slice(0, 200);
      if (this.AllUser.length < perPage || page==10) {
        break; 
      }
      page++; 
    }
    console.log(this.AllUser);
  } catch (error) {
    console.error(error);
  }
},
},
  data(){
    return{
      AllUser:[],
      search:""
    }
  },
  computed:{
         mysearch(){
            var f=new RegExp(this.search,'i');
            return this.AllUser.filter(function(el){
                  return el.login.match(f);
            })
         },
    }
  }
</script>


<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
