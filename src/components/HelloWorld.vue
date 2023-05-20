<template>
  <div class="container">
    <div class="mt-5">
      <input type="text" v-model="search" class="form-control" placeholder="serach">
    </div>
       <div class="mt-3">
           <table class="table table-stripped">
            <thead class="text-center">
               <th>
                  <td>#</td>
               </th>
                <th>
                  <td>Name</td>
                </th>
                <th>
                  <td>Image</td>
                </th>
              </thead>
              <tbody>
                 <tr v-for="(user,index) in mysearch" :key="user.id">
                  <td>{{index+1}}</td>
                    <td>{{user.login}}</td>
                    <td><img width="100px" :src="user.avatar_url"></td>
                    <!-- <td>{{user.commitCount}}</td> -->
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
     //this.fetchUserRankings();
  },
  methods:{
    async  fetchAllUsers() {
    
  const currentDate = new Date();
  const sinceDate = currentDate.toISOString();
  let page = 1;
  const perPage = 100;
 

  // Add cache control headers to disable caching
  const headers = {
    'Accept': 'application/vnd.github.v10+json',
    'Authorization': 'Bearer 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',
    //'Cache-Control': 'no-cache',
    //'Pragma': 'no-cache',
  };
      /*const delay = (ms) => new Promise((resolve) => setTimeout(resolve, ms));
      const requestDelay = 500;*/

     // https://github.com/gayanvoice/top-github-users
      const response = await axios.get('https://api.github.com/search/users', {
        params: {
            q: 'location:Tunisia type:user',
            sort: 'commits',
           // sort:"repositories",
            //sort:"contributions",
            order: 'desc',
            per_page: 100,
            /*page:1,*/
            since:sinceDate
        },
        headers:headers
      });
           // await delay(requestDelay); 
      //this.AllUser = this.AllUser.concat(response.data.items);
      this.AllUser=response.data.items;
      //  if (this.AllUser.length < perPage || page==10) {
      //    break; 
      //  }
      //  page++;
      
    /* //q:"TalelMejri"
          q: 'location:Tunisia',
          per_page: perPage,
          sort:"commits",
          //sort:"contributions",
          page,*/
},
async fetchUserRankings() {
      try {
        const currentDate = new Date();
        const sinceDate = currentDate.toISOString();

        const response = await axios.get('https://api.github.com/search/users', {
          params: {
            q: 'location:Tunisia type:user',
            sort: 'commits',
            order: 'desc',
            per_page: 100,
            since: sinceDate,
          },
        });

        this.AllUser = response.data.items;
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
