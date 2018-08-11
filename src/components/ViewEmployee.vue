<template>
  <div id="view-employee">
    <ul class="collection with-header">
      <li class="collection-header"><h5>{{name}}</h5></li>
      <li class="collection-item"><p> Employee Id: {{employee_id}}</p></li>
      <li class="collection-item"><p> Department: {{dept}}</p></li>
      <li class="collection-item"><p>Position: {{position}}</p></li>
    </ul>

   <router-link to='/' class="btn">Go Back</router-link>
   <button @click="deleteEmployee" class="btn red">Delete</button>

<div class="fixed-action-btn">
      <router-link v-bind:to="{name: 'edit-employee',params: {employee_id: employee_id}}" class="btn-floating btn-large green">
        <i class="fa fa-pencil"></i>
      </router-link>
    </div>

  </div>
</template>
<script>
import db from './firebaseInit'
  export default {
    name: 'view-employee',
    data(){
      return {
        emp_id: null,
        name: null,
        dept: null,
        position: null
      }
    },
    beforeRouteEnter (to, from, next) {
      db.collection('emp').where('emp_id', '==',
       to.params.employee_id).get()
      .then(querySnapshot =>{
        querySnapshot.forEach(doc =>{
          next(vm=>{
            vm.employee_id =doc.data().emp_id
            vm.name= doc.data().name
            vm.dept = doc.data().dept
            vm.position = doc.data().position
          })
        })
      })
    },
    watch: {
      '$route': 'fetchData'
    },
    methods: {
      fetchData(){
        db.collection('emp').where('emp_id', '==',
        this.$route.params.employee_id).get()
        .then(querySnapshot =>{
        querySnapshot.forEach(doc =>{
            this.employee_id =doc.data().emp_id
            this.name= doc.data().name
            this.dept = doc.data().dept
            this.position = doc.data().position
             })
          })
        },

        deleteEmployee(){
          if(confirm('Are you sure?')){
            db.collection('emp').where('emp_id', '==',
        this.$route.params.employee_id).get()
        .then(querySnapshot =>{
        querySnapshot.forEach(doc =>{
            doc.ref.delete()
            this.$router.push('/')
             })
          })
        }
     }
  }
}
</script>
