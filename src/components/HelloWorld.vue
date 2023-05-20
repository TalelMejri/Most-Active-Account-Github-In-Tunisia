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
  },
  methods:{
    async  fetchAllUsers() {
      const currentDate = new Date();
      const firstDayOfMonth = new Date(currentDate.getFullYear(), currentDate.getMonth(), 1);
      const sinceDate = firstDayOfMonth.toISOString();
      let page = 1;
      const perPage = 100;
      /*const delay = (ms) => new Promise((resolve) => setTimeout(resolve, ms));
      const requestDelay = 500;*/
 
   
      const response = await axios.get('https://api.github.com/search/users', {
        params: {
         /* //q:"TalelMejri"
          q: 'location:Tunisia',
          per_page: perPage,
          sort:"commits",
          //sort:"contributions",
          page,*/
            q: 'location:Tunisia type:user',
           // sort: 'commits',
           // sort:"repositories",
            sort:"contributions",
            order: 'desc',
            per_page: 100,
            since: sinceDate,
            page:1
        },
        headers: {
          Accept: 'application/vnd.github.v10+json',
          Authorization: 'Bearer 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',
        },
      });
     // await delay(requestDelay); 
      this.AllUser = this.AllUser.concat(response.data.items).slice(0, 1000);
      // if (this.AllUser.length < perPage || page==10) {
      //   break; 
      // }
     // page++;

    // this.getUsersCommitCounts(); 

},
    // getUsersCommitCounts() {
    //   this.AllUser.forEach((user) => {
    //     Axios
    //       .get(`https://api.github.com/users/${user.login}/repos`, {
    //         headers: {
    //            Accept: 'application/vnd.github.v3+json',
    //            Authorization: 'Bearer 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',
    //        },
    //       })
    //       .then((response) => {
    //         const repos = response.data;
    //         let commitCount = 0;
          
    //         repos.forEach((repo) => {
    //           commitCount += repo.commits;
    //         });
    //         console.log(commitCount);
    //         user.commitCount = commitCount;
    //       })
    //       .catch((error) => {
    //         console.error(error);
    //       });
    //   });
    // },
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
