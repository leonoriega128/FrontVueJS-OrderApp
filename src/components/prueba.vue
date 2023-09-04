<script setup>
import Button from 'primevue/button';
import InputText from 'primevue/inputtext';
import {ref } from 'vue';
import axios from 'axios';

     const  formData= ref({
        courseCode: 0,
        dischargeDate: "",
        collegeName: "",
        collegeLevel: "",
        collegeCourse: "",
        schoolLocation: "",
        results: [
          {
            numberGraduates: 0,
            itemName: "",
            price: 0,
            total: 0
          }
        ],
        total: 0,
        deliveryDate: ""
      });
    var  contractId = null;
    const  contractData = ref(null);
    var mensajeVisible = ref(false);
    var  mensaje = "";
  
  
  

 const fetchContractData= () =>{
    if(contractId==null){
        mensajeVisible = true;
        mensaje = "Debe ingresar el codigo de un curso.";
    } else{
       
      axios.get('https://localhost:44323/api/v1/Course/InfoContrato?codeCourse='+contractId)
      .then((response) => {
        if(response.data != null){
            mensajeVisible = false;
            mensaje="";
            contractId = null;
            contractData.value = response.data;
            formData.value = response.data;
      }
        
      })
      .catch((error) => {
        console.error('Error al obtener los datos:', error);
      });
    
    }};
  
  

</script>

<template>
 <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
</head>

<div class="form-search flex align-content-center">  
    <form @submit.prevent="fetchContractData()" class="flex align-content-center">
          <label for="contractId">Codigo de curso: </label>
          <InputText type="text" id="contractId" v-model="contractId" />
          <Button type="submit">Consultar contrato</Button>
          <div v-if="mensajeVisible.valueOf" class="alert alert-success" role="alert" >
          {{ mensaje }}
        
         </div>
    </form>
    <div id="app" Class="containerContr">
        <h4>Información del Contrato</h4>
      
    
           <form v-if="contractData" >
               
            <div class="form-group">
          <label for="courseCode">Código del Curso: </label>
          <InputText type="textArea" id="courseCode" v-model="contractData.courseCode" readonly="true"/>
           <label for="dischargeDate">Fecha de alta: </label>
          <InputText type="textArea" id="dischargeDate" v-model="formData.dischargeDate" readonly="true"/> <br>
          <label for="collegeName">Colegio: </label> 
          <InputText type="textArea" id="collegeName" v-model="formData.collegeName" readonly="true"/> 
          <label for="collegeLevel">Nivel: </label>
          <InputText type="textArea" id="collegeLevel" v-model="formData.collegeLevel" readonly="true"/> <br>
          <label for="collegeCourse">Curso: </label>
          <InputText type="textArea" id="collegeCourse" v-model="formData.collegeCourse" readonly="true"/> 
          <label for="schoolLocation">Localidad: </label>
          <InputText type="textArea" id="schoolLocation" v-model="formData.schoolLocation" readonly="true"/> 
          
            </div>
            <div Class="fondosRes">
            <h6>Resultados</h6>
      <div v-for="(value, key) in formData.results" :key="key" class="panel">
       <label for="numberGraduates">Cantidad: </label>
      <InputText type="text" id="numberGraduates" v-model="value.numberGraduates"/>
      <label for="itemName">Articulo: </label>
      <InputText type="text" id="itemName" v-model="value.itemName"/>
      <label for="price">Precio unitario: </label>
      <InputText type="text" id="price" v-model="value.price"/>
      <label for="total">Total:</label>
      <InputText type="text" id="total" v-model="value.total"/>  
      </div>
      </div>
      
      </form>
 
        </div>
</div>


</template>
  


