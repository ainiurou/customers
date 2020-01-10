<template>
  <div class="customers container">
      <Alert v-if="alert" v-bind:message="alert"></Alert>
  <h1 class="page-header">用户管理系统</h1>

    <input type="text" class="form-control" placeholder="搜索" v-model="filterInput">
    <br>
  <table class="table table-striped">
      <thead>
          <tr>
              <th>姓名</th>
              <th>电话</th>
              <th>邮箱</th>
              <th></th>
          </tr>
      </thead>
      <tbody>
          <tr v-for ="customer in filterBy(customers,filterInput)">
              <td>{{customer.name}}</td>
              <td>{{customer.phone}}</td>
              <td>{{customer.email}}</td>
              <td><router-link class="btn btn-default" v-bind:to ="'/customer/'+ customer.id">详情</router-link></td>
          </tr>
      </tbody>

  </table>
  </div>
</template>

<script>
import Alert from './Alert'
export default {
  name: 'customers',
  data () {
    return {
        customers:[],
        alert:"",
        filterInput:""
    }
  },
    methods:{
        fetchCustomers(){
            this.$http.get("http://localhost:3000/users")
                .then(function(response){
                    // console.log(response);
                    this.customers=response.body;
                })
        },
        filterBy(customers,value){
            return customers.filter(function(customer){
                return customer.name.match(value);
            })
        }       //filter ES6中的一个方法
        
    },
     //query和params都是在VUE路由中传参，
     //用法：query用path来引入，params只能用name来传递，不能使用path，
     //展示效果:query更像ajax中get请求（会在地址栏显示参数），而params更像post方式传递（不会在地址栏显示参数）
    created(){
        if(this.$route.query.alert){
            this.alert=this.$route.query.alert
        }
            this.fetchCustomers();

        },
    updated(){
            this.fetchCustomers();
    },
    components:{
        Alert
    }

  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
