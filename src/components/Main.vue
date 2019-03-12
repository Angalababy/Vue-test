<template>
  <div>
    <h2 v-if="firstView">请输入关键字进行搜索</h2>
    <h2 v-else-if="loading">请求中</h2>
    <h2 v-else-if="errorMsg">{{errorMsg}}</h2>
  <div class="row" v-else>
    <div class="card" v-for="(user,index) in users" :key="index">
      <a :href="user.url" target="_blank">
        <img :src="user.avatar_url" style='width: 100px'/>
      </a>
      <p class="card-text">{{user.username}}</p>
    </div>
  </div>
  </div>
</template>
<script>
  import pubsub from 'pubsub-js'
  import axios from 'axios'
  export default({
    data(){
      return{
        firstView:true,
        loading:false,
        users:[],
        errorMsg:''
      }
    },
    mounted(){
      pubsub.subscribe('search',async(msg,searchName)=>{
        this.firstView=false
        this.loading=true
        const url=`https://api.github.com/search/users?q=${searchName}`
      try{
        const response=await axios.get(url)
          //更新状态
          const result=response.data
          const users=result.items.map(item=>({
            username:item.login,
            avatar_url:item.avatar_url,
            url:item.html_url,
          }))
          this.loading=false
          this.users=users
      }catch (error){
          this.loading=false
          this.errorMsg='请求失败'
        }

      })

    }
  })
</script>
<style>
  .card {
    float: left;
    width: 33.333%;
    padding: .75rem;
    margin-bottom: 2rem;
    border: 1px solid #efefef;
    text-align: center;
  }

  .card > img {
    margin-bottom: .75rem;
    border-radius: 100px;
  }

  .card-text {
    font-size: 85%;
  }
</style>
