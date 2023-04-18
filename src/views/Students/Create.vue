<template>
  <div class="container mt-5">
    <div class="card">

      <div class="card-header">
        <h4>Add Students</h4>
      </div>
      <div class="card-body">
        <span v-if="errorList">
        <ul class="alert alert-warning" v-if="Object.keys(errorList.length > 0)">
          <li class="mb-0 ms-3" v-for="(error,index) in errorList" :key="index">
         {{ error[0] }}
          </li>
        </ul>
      </span>
        <div class="mb-3">
          <label for="">Name</label>
          <input v-model="model.student.name" type="text" class="form-control" />
        </div>
        <div class="mb-3">
          <label for="">Course</label>
          <input v-model="model.student.course" type="text" class="form-control" />
        </div>
        <div class="mb-3">
          <label for="">Email</label>
          <input v-model="model.student.email" type="text" class="form-control" />
        </div>
        <div class="mb-3">
          <label for="">Phone</label>
          <input v-model="model.student.phone" type="text" class="form-control" />
        </div>
        <div class="mb-3">
          <button type="button" @click="saveStudent()" class="btn btn-primary">Save</button>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
import { resolveComponentType } from '@vue/compiler-core';
import axios from 'axios';

export default {
  name: 'studentCreate',
  data() {
    return {
      errorList:null,
      model: {
        student: {
          name: '',
          course: '',
          email: '',
          phone: '',
        }
      }
    }
  },
  methods: {

    saveStudent() {
      var mythis=this;
      axios.post('http://127.0.0.1:8000/api/students/', this.model.student).then(res => {
        console.log(res.data);
        alert(res.data.message);

        this.model.student = {
          name: '',
          course: '',
          email: '',
          phone: '',
        }
        this.errorList='';
      }).catch(function(error){
        if(error.response)
        {
          if(error.response.data.status==422)
          {
            console.log("IF:"+error.response.data.errors)
            mythis.errorList=error.response.data.errors;
          }
        }
        else if(error.request)
        {
          console.log(error.request)
        }
        else
        {
          console.log('Error',error.message)
        }
      })
    }

  },
}
</script>