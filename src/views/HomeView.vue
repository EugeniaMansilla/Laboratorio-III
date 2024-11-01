<template>
  <div class="frente">
    <header>
      <h1>- - - MotosGD - - -</h1>
    </header>

    <main>
      <section>
        <h2>Cargar nuevo modelo</h2>
        <form @submit.prevent="agregarProducto">
          <p>Marca</p>
          <input 
            v-model="newProducto.marca" 
            placeholder="Agregar"
            required
          />
          <p>Modelo</p>
          <input 
            v-model="newProducto.modelo" 
            placeholder="Agregar" 
            required 
          />
          <br><br>
          <button> Agregar </button>
        </form><br>

        <table>
          <thead>
            <tr>
              <th>Código</th>
              <th>Marca</th>
              <th>Modelo</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(producto) in productos" :key="producto.idcod">
              <td>{{ producto.idcod }}</td>
              <td>{{ producto.param1 }}</td>
              <td>{{ producto.param2 }}</td>
              <td>
                <!--button><router-link :to="{name:'EditarView', params: {id: producto.idcod}}">
                   Editar </router-link></button-->
                <button @click="vistaEditar(producto.idcod)">Editar</button>
                <button @click="eliminarProducto(producto.idcod)">Eliminar</button>
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
const APIproducto = 'https://api.yumserver.com/13678/generic/producto';

export default {
  data() {
    return {
      productos: [],
      newProducto: { 
        marca: '', 
        modelo: '' },
    };
  },
  methods: {
    vistaEditar(id) {
      this.$router.push({ name: 'EditarView', params: { id } });
    },
    async leerTabla() {
      try {
        const response = await axios.get(APIproducto)
        this.productos = [...response.data]
      } catch (error) {
        console.log(error);
      }
    },
    async agregarProducto() {
      if (!this.newProducto.marca.trim() || !this.newProducto.modelo.trim()) {
        this.mensajeError = 'Todos los campos son requeridos';
       return;
      }
      try {
        const producto ={
          param1: this.newProducto.marca,
          param2: this.newProducto.modelo,

        }
        await axios.post(APIproducto, producto);
        alert('Producto agregado correctamente.')

        this.newProducto.marca = '';
        this.newProducto.modelo = '';
        await this.leerTabla();
      } catch (error) {
        console.log(error);
      }
    },
    async eliminarProducto(i) {
      try {
      const confirmacion = confirm("¿Estás seguro de que deseas eliminar este producto?");

      if (confirmacion) {
        const prodEliminar = {
          idcod: i,
        };
        
        await axios.delete(APIproducto, { data: prodEliminar });
        await this.leerTabla();
        
        alert('Producto eliminado correctamente.');
      } else {
        alert('Eliminación cancelada.');
      }
    } catch (error) {
      console.log(error);
      alert('No se pudo eliminar el producto.');
    }
    },
  }, 
    async mounted() {
    await this.leerTabla();
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
  background-color: gray;
  color: white;
  font-size: 16px;
  cursor: pointer;
  border-radius: 5px;
  transition: background-color 0.3s ease;
  margin: 5px;
}

button:hover {
  background-color: #458dda;
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
  background-color: rgb(5, 5, 191);
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



