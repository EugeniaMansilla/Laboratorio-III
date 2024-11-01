<template>
    <div class="inf">
        <h1>Datos del producto</h1>
          <div v-if="producto !== null"> 
            <ul>
                <li> <p>Nombre:</p> {{ producto.param1 }} </li>
                <li> <p>Modelo:</p> {{ producto.param2 }}</li>
            </ul>
            <h1>Editar Producto</h1>
            <input type="text" v-model="editProducto.nombre" placeholder="Nuevo nombre" />
            <input type="text" v-model="editProducto.categoria" placeholder="Nueva categoría" />
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
                        nombre: '', 
                        categoria: '' },
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
                try {
                const prodActualizado = {
                    idcod: producto.idcod,
                    param1: this.editProducto.nombre,
                    param2: this.editProducto.categoria,
                };
                
                await axios.patch(`https://api.yumserver.com/13678/generic/producto`,prodActualizado,);
                
                alert('Producto actualizado correctamente.');
                this.editProducto.nombre = "";
                this.editProducto.categoria = "";
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



    
 
