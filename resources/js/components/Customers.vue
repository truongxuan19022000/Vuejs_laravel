<template>
 <div class="container">
   <div class="row justify-content-center">
     <div class="col-md-8">
       <div class="card">
         <div class="card-header">Customer</div>
          <form @submit.prevent="editCustomer" method="post">
            <!-- su kien submit tranh load toi trang khac -->
            <div class="form-group" > 
              <label for="exampleInputEmail1">Name</label>
              <input type="text" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" placeholder=" Name" v-model="customer.name_customer">
            </div>
            <div class="form-group">
              <input type="text" class="form-control" id="exampleInputPassword1" placeholder="Phone Number" v-model="customer.phone_customer">
            </div>
            <div class="form-group">
              <input type="text" class="form-control" id="exampleInputPassword1" placeholder="Address" v-model="customer.address_customer">
            </div>
            <div class="form-group">
              <input type="text" class="form-control" id="exampleInputPassword1" placeholder="Email" v-model="customer.email_customer">
            </div>
            <div class="form-group">
              <input type="text" class="form-control" id="exampleInputPassword1" placeholder="city" v-model="customer.city_customer">
            </div>
            
            <button type="submit"  class="btn btn-primary">Submit</button>
          </form>
          
         <div class="card-body">
            <table class="table"  >
              <thead>
                <tr>
                  <th scope="col">Id</th>
                  <th scope="col">Name</th>
                  <th scope="col">Phone</th>
                  <th scope="col">Address</th>
                  <th scope="col">Function</th>
                </tr>
              </thead>
              <div class="text-center">
                  <div class="spinner-border" role="status">
                    <span class="sr-only">Loading...</span>
                  </div>
                </div> 
              <tbody v-for="(customer,id_customer) in customers" :key="id_customer">
                <tr>
                  <th scope="row">{{ customer.id_customer }} </th>
                  <td>{{ customer.name_customer }}</td>
                  <td>{{ customer.phone_customer }}</td>
                  <td>{{ customer.address_customer }}</td>
                  <td class="Button">
                    <button type="button" class="btn btn-success" @click="say()">Edit</button>
                    <button type="button" class="btn btn-danger" @click="DeleteCustomer(customer.id_customer)" >Delete</button>
                  </td>
                </tr>
              </tbody>
            </table>
         </div>
         <nav aria-label="Page navigation example">
            <ul class="pagination">
              <li class="page-item" >
                <a class="page-link" href="#" @click="fetchCustomers(pagination.prev_page_url)" aria-label="Previous">
                  <span aria-hidden="true">&laquo;</span>
                  <span class="sr-only">Previous</span>
                </a>
              </li>
              <li class="page-item"><a class="page-link" @click="fetchCustomers(pagination.first_page)" href="#">1</a></li>
              <li class="page-item"><a class="page-link" @click="fetchCustomers(pagination.next_page_url)" href="#">2</a></li>
              <li class="page-item"><a class="page-link" @click="fetchCustomers(pagination.last_page)"  href="#">3</a></li>
              <li class="page-item" >
                <a class="page-link" href="#"  @click="fetchCustomers(pagination.next_page_url)"  aria-label="Next">
                  <span aria-hidden="true">&raquo;</span>
                  <span class="sr-only">Next</span>
                </a>
              </li>
            </ul>
          </nav>
       </div>
     </div>
   </div>
 </div>
</template>

<script>
export default {
name:'Example',
data(){
  return{
    customers:[],
    customer:{
      id_customer:'',
      name_customer:'',
      phone_customer:'',
      address_customer:'',
      email_customer:'',
      city_customer:''
    },
    id_customer:'',
    pagination:{}
  }
},
created(){
  this.fetchCustomers();
},
methods:{
  fetchCustomers:function(page_url){
    let vm=this;
    page_url= page_url || 'api/customers'
    fetch(page_url)
    .then(res => res.json())
    .then(res => {
      this.customers=res.data;
      vm.makePagination(res.meta,res.links);
    })
    .catch(error => 'error');
  },
  
  DeleteCustomer:function(id){
    if(confirm('Bạn muốn xóa ?')){
      axios.delete(`api/customers/${id}`)
      .then(res => {
        alert('Delete ok');
        this.fetchCustomers();
      })
      .catch(err =>console.log(err));
      
    }
  },
  makePagination:function(meta,links){ //ham phan trang theo link 
    let pagination={
      current_page:meta.current_page,//trang dau
      last_page:meta.last_page,//trang cuoi
      first_page :links.first,
      last_page:links.last,
      next_page_url :links.next,
      prev_page_url:links.prev
    }
    this.pagination= pagination;
  },
  editCustomer:function(){
    if(this.edit === false){
      //edit
    }
    else{
      
      var formData = new FormData();
      formData.append('name_customer',this.customer.name_customer);
      formData.append('phone_customer',this.customer.phone_customer);
      formData.append('address_customer',this.customer.address_customer);
      formData.append('email_customer',this.customer.email_customer);
      formData.append('city_customer',this.customer.city_customer);
       axios.post('api/customers',formData)
      .then(res =>{
        alert('them thanh cong!');
        this.customer.name_customer = "";
        this.customer.phone_customer = "";
        this.customer.address_customer = "";
        this.customer.email_customer = "";
        this.customer.city_customer = "";
        this.fetchCustomers();
      })
      .catch(err => console.log(err));
    }
  }
}
}
</script>

<style>
.Button{
  display: flex;
  margin-right:5px ;
}
</style>