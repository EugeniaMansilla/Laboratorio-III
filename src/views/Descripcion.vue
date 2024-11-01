<template>
<div class="inf">
    <h1>Información del cliente</h1>
      <div v-if="cliente !== null"> 
        <ul>
            <li> <p>id:</p> {{ cliente.id }} </li>
            <li> <p>Nombre:</p> {{ cliente.param1 }}</li>
            <li> <p>Apellido:</p> {{ cliente.param2 }}</li>
            <li> <p>Dirección:</p> {{ cliente.param3 }}</li>
            <li> <p>Teléfono</p> {{ cliente.param4 }}</li>
            <li> <p>Correo electrónico:</p> {{ cliente.param5 }}</li>
        </ul>
      </div>
    </div>
</template>

<script>
import axios from 'axios';

    export default {
        name: 'DescripcionView',
        props: {
            id:{
                type: String,
                required: true,
            }
        },
        data() {
            return {
                cliente: null,
             }
           
        },
        async created() {
            try{
                const responde = await axios.get('https://api.yumserver.com/13678/generic/cliente/' + this.id);
                this.cliente = responde.data;
            } catch (error) {
                alert('No hay descripcion');
            }
        },
    }
 
</script>

<style>
.inf {
    background-color: #f9f9f9; 
    border: 1px solid #ddd; 
    border-radius: 8px; 
    padding: 20px; 
    max-width: 500px; 
    margin: 20px auto; 
    
  }
  
  .inf h1 {
    font-size: 1.5rem; 
    color: black; 
    text-align: center; 
    margin-bottom: 20px; 
  }
  

  .inf li {
    font-size: 1rem; 
    color: black; 
    margin: 25px 0; 
 
  }
  
  .inf p {
    display: inline;
    font-weight: bold; 
    color: black; 
  }
  </style>
  