<template>
    <div class="container mt-5">
      <div class="card">
  
        <div class="card-header">
          <h4>Edit Student</h4>
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
            <button type="button" @click="updateStudent()" class="btn btn-primary">Update</button>
          </div>
        </div>
  
      </div>
    </div>
  </template>
  
  <script>
  import { resolveComponentType } from '@vue/compiler-core';
  import axios from 'axios';
  
  export default {
    name: 'studentEdit',
    data() {
      return {
        studentId:'',
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
    mounted()
    {
          console.log(this.$route.params.id);
          this.studentId=this.$route.params.id;
          this.getStudentData(this.$route.params.id);
    },
    methods: {
      getStudentData(studentId)
      {
        var mythis=this;
        axios.get(`http://127.0.0.1:8000/api/students/${this.studentId}/edit`).then(res=>{
                console.log(res.data.student);
                this.model.student=res.data.student;
            }).catch(function(error){
          if(error.response)
          {
            console.log("IF 1")
            if(error.response.data.status==404)
            {
              console.log("IF 2")
              //mythis.errorList=error.response.data.message;
              alert(error.response.data.message);
            }
          }
          else
          {
            console.log('Error',error.message)
          }
        });
      },
      updateStudent() {
        var mythis=this;
        axios.put(`http://127.0.0.1:8000/api/students/${this.studentId}/edit`, this.model.student).then(res => {
          console.log(res.data);
          alert(res.data.message);
          this.errorList='';
        }).catch(function(error){
          if(error.response)
          {
            if(error.response.data.status==422)
            {
              mythis.errorList=error.response.data.errors;
            }
            if(error.response.data.status==404)
            {
              alert(error.response.data.message);
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