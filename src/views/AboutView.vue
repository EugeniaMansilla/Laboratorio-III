<template>
    <div class="frente">
      <header>
        <h1>- - - Clientes - - -</h1>
      </header>
  
      <main>
        <section>
          <h2>Cargar nuevo cliente</h2>
          <form @submit.prevent="agendarCliente">
            <p>Nombre   <input
              v-model="newCliente.nombre" 
              placeholder="Agregar" 
              required />
            </p>
            <p>Apellido   <input
              v-model="newCliente.apellido" 
              placeholder="Agregar" 
              required />
            </p>
            <p>Dirección   <input
              v-model="newCliente.direccion" 
              placeholder="Agregar" 
              required />
            </p>
            <p>Teléfono   <input
              v-model="newCliente.telefono" 
              placeholder="Agregar" 
              required />
            </p>
            <p>Correo electrónico   <input
              v-model="newCliente.correo" 
              placeholder="Agregar" 
              required />
            </p>
            <button> Agendar </button>
          </form><br>
  
          <table>
            <thead>
              <tr>
                <th>id</th>
                <th>Clientes</th>
                <th>Información</th>
                <th></th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(cliente) in clientes" :key="cliente.idcod">
                <td>{{ cliente.id }}</td>
                <td>{{ cliente.param1 }}  {{ cliente.param2 }}</td>
                <td><router-link :to="{name:'DescripcionView', params: {id: cliente.idcod}}"
                  > ver + </router-link></td>
                <td>
                  <button @click="editarCliente(cliente)">Editar</button>
                  <button @click="eliminarCliente(cliente.idcod)">Eliminar</button>
                </td>
              </tr>
            </tbody>
          </table>
        </section>
      </main>
  
      <footer>
        <p>motogd@gmail.com </p>
        <p>3564-509998</p>
      </footer>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  const APIcliente = 'https://api.yumserver.com/13678/generic/cliente';
  
  export default {
    data() {
      return {
        clientes: [],
        newCliente: { 
          nombre: '', 
          apellido: '' ,
          direccion: '', 
          telefono: '',
          correo:''},
      };
    },
    methods: {
      
      async leerTablaCliente() {
        try {
          const response = await axios.get(APIcliente)
          this.clientes = [...response.data]
        } catch (error) {
          console.log(error);
        }
      },
  
      async agendarCliente() {
        if (this.newCliente.nombre.trim().length === 0 || this.newCliente.apellido.trim().length === 0 ||
            this.newCliente.direccion.trim().length === 0 || this.newCliente.telefono.trim().length === 0 ||
            this.newCliente.correo.trim().length === 0) {
            alert('Todos los campos son requeridos.');
        return;
        }
        try {
          const cliente ={
            param1: this.newCliente.nombre,
            param2: this.newCliente.apellido,
            param3: this.newCliente.direccion,
            param4: this.newCliente.telefono,
            param5: this.newCliente.correo,
            
  
          }
          await axios.post(APIcliente, cliente);
  
           
          this.newCliente.nombre = '';
          this.newCliente.apellido = ''
          this.newCliente.direccion = '';
          this.newCliente.telefono = '';
          this.newCliente.correo = '';
         
          await this.leerTablaCliente();
        } catch (error) {
          console.log(error);
        }
      },
      async eliminarCliente(i) {
        try {
        const confirmacion = confirm("¿Estás seguro de que deseas eliminar este cliente?");
  
        if (confirmacion) {
          const prodEliminar = {
            idcod: i,
          };
          
          await axios.delete(APIcliente, { data: prodEliminar });
          await this.leerTablaCliente();
          
          alert('Cliente eliminado correctamente.');
        } else {
          alert('Eliminación cancelada.');
        }
      } catch (error) {
        console.log(error);
        alert('No se pudo eliminar el cliente.');
      }
      },
      async editarCliente(cliente) {
      try {
      const nuevoN = prompt("Ingrese nuevo nombre:", cliente.param1);
      const nuevoA = prompt("Ingrese nuevo apellido:", cliente.param2);
      const nuevoD = prompt("Ingrese nueva dirección:", cliente.param3);
      const nuevoT = prompt("Ingrese nuevo teléfono:", cliente.param4);
      const nuevoC = prompt("Ingrese nuevo correo electrónico:", cliente.param5);

  
      if (nuevoN && nuevoA && nuevoT && nuevoD && nuevoC) {
        const clienteActualizado = {
          idcod: cliente.idcod,
          param1: String(nuevoN),
          param2: String(nuevoA),
          param3: String(nuevoD),
          param4: String(nuevoT),
          param5: String(nuevoC),
        };
      
        await axios.patch(APIcliente, clienteActualizado,);
      
        await this.leerTablaCliente();
        alert('Cliente actualizado correctamente.');
      }
    } catch (error) {
      console.error("Error al actualizar cliente:", error);
      alert('No se pudo actualizar cliente.');
    }
      },
  
      
      
    }, 
      async mounted() {
      await this.leerTablaCliente();
    }
  }
  </script >
  
  <style scoped>

  body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #f5f5f5;
    min-height: 100vh;
  
  }
  
  .frente {
    width: 100%;
    max-width: 800px;
    border-radius: 10px;
    overflow: hidden;
    background-color: #ffffff;
    margin: auto;
    border: 1px solid #ddd; 
      
  }
    
  header,
  footer {
    background-color: black;
    color: white;
    text-align: center;
    padding: 15px 0;
  }
  
  header h1,
  footer p {
    margin: 0;
  }
  
  main {
    padding: 20px;
  }
  
  h2 {
    color: black;
    margin-bottom: 15px;
  }
  
  form p {
    font-weight: bold;
    color: black;
  }
  
  input {
    width: 50%;
    padding: 12px;
    margin: 5px 0 15px;
    border: 1px solid #ddd;
    border-radius: 5px;
    outline: none;
    box-sizing: border-box;
    transition: border-color 0.3s ease;
  }
  
  input:focus {
    border-color:blue;
  }
  
  button {
    border: none;
    padding: 10px 10px;
    background-color: grey;
    color: white;
    font-size: 16px;
    cursor: pointer;
    border-radius: 5px;
    transition: background-color 0.3s ease;
    margin: 5px;
  }
  
  button:hover {
    background-color: plum;
  }
  
  table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
    box-shadow: 0 2px 4px black(0, 0, 0, 0.1);
    border-radius: 5px;
    overflow: hidden;
  }
  
  table thead {
    background-color: purple;
    color: white;
  }
  
  table th,
  table td {
    padding: 12px;
    text-align: left;
    border-bottom: 1px solid #ddd;
  }
  table th {
    font-weight: bold;
  }
  
  table td:last-child {
    text-align: center;
  }
  
  footer p {
    font-size: 14px;
    margin: 0;
  }
  
  footer {
    font-size: 14px;
    padding: 10px 0;
  }
  </style>
  
  
  
  

