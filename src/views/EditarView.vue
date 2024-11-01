<template>
    <div class="inf">
        <h1>Datos del producto</h1>
          <div v-if="producto !== null"> 
            <ul>
                <li> <p>Marca:</p> {{ producto.param1 }} </li>
                <li> <p>Modelo:</p> {{ producto.param2 }}</li>
            </ul>
            <h1>Editar Producto</h1>
            <input type="text" v-model="editProducto.marca" placeholder="Nueva Marca" required/>
            <input type="text" v-model="editProducto.modelo" placeholder="Nuevo Modelo" required/>
            <button @click="guardarEdicion(producto)">Guardar Cambios</button>
          </div>
        </div>
        
    </template>
    
    <script>
    import axios from 'axios';
    
        export default {
            name: 'EditarView',
            props: {
                id:{
                    type: String,
                    required: true,
                }
            },
            data() {
                return {
                    producto: null,
                    editProducto: { 
                        marca: '', 
                        modelo: '' },
                 }
               
            },
            async created() {
                await this.cargarProducto();
            },

            methods: {
                async cargarProducto() {
                try {
                    const response = await axios.get(`https://api.yumserver.com/13678/generic/producto/${this.id}`);
                    this.producto = response.data;
                } catch (error) {
                    alert('No hay descripción');
                }
                },
                async guardarEdicion(producto) {
                    if (this.editProducto.marca.trim().length === 0 || this.editProducto.modelo.trim().length === 0) {
                    alert('Todos los campos son requeridos.');
                    return;
                } 
                try {
                const prodActualizado = {
                    idcod: producto.idcod,
                    param1: this.editProducto.marca,
                    param2: this.editProducto.modelo,
                };
                
                await axios.patch(`https://api.yumserver.com/13678/generic/producto`,prodActualizado,);
                
                alert('Producto actualizado correctamente.');
                this.editProducto.marca = "";
                this.editProducto.modelo = "";
                await this.cargarProducto()

                
            } catch (error) {
                console.error("Error al editar el producto:", error);
                alert('No se pudo editar el producto.');
            }
                },

        },
        }
    </script>

    <style>
        input {
        width: 30%;
        padding: 10px;
        margin: 5px;
        border: 1px solid #ddd;
        border-radius: 5px;
        outline: none;
        box-sizing: border-box;
        transition: border-color 0.3s ease;
        }
        button {
        border: none;
        padding: 10px;
        background-color: gray;
        color: white;
        font-size: 15px;
        cursor: pointer;
        border-radius: 5px;
        margin: 5px;
        }
    </style>



    
 
