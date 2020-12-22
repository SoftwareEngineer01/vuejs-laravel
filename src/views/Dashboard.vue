<template>
    <div>
        <Header />

        <div class="container my-3">

            <h1 class="my-3">Listado de Clientes</h1>

             <button class="btn btn-primary my-3 float-left" v-on:click="nuevo()">Nuevo</button>

            <table class="table table-hover table-responsive">
                <thead>
                    <tr>
                        <th scope="col">#</th>
                        <th scope="col">Nombre</th>
                        <th scope="col">Apellidos</th>
                        <th scope="col">Identificación</th>
                        <th scope="col">Teléfono</th>
                        <th scope="col">Opciones</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="cliente in ListadoClientes" :key="cliente.id">
                        <th scope="row">{{cliente.id}}</th>
                        <td>{{cliente.name}}</td>
                        <td>{{cliente.surname}}</td>
                        <td>{{cliente.identification}}</td>
                        <td>{{cliente.telephone}}</td>
                        <td>
                            <button type="button" class="btn btn-warning mx-1" v-on:click="editar(cliente.id)">Editar</button>
                            <button type="button" class="btn btn-danger mx-1" v-on:click="deleteClient(cliente.id)">Eliminar</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <Footer />
    </div>
</template>

<script>
    import Header from '@/components/Header.vue';
    import Footer from '@/components/Footer.vue';
    import axios from 'axios'

    export default {
        name: "Dashboard",
        data(){
            return {
                ListadoClientes : null
            }
        },
        components: {
            Header,
            Footer
        },
        methods: {
            getClients() {
                let url = "http://localhost:8100/api/clientes";
                axios
                .get(url)
                .then(data => {
                    this.ListadoClientes = data.data.data;
                })
                .catch(error => console.log(error))
            },
            nuevo() {
                this.$router.push('/nuevo');
            },
            editar(id){
                this.$router.push('/editar/'+id);
            },
            deleteClient(id){
                if (confirm('Esta seguro de eliminar el registro?')) {
                    axios
                    .delete('http://localhost:8100/api/cliente/'+id)
                    .then( response => {
                        if (response.status == 200) {
                            this. makeToast('Realizado', 'Cliente eliminado', 'danger'),
                            this.getClients();
                        }else {
                            console.log('Error al eliminar el registro');
                        }
                    })
                    .catch(error => console.log(error))
                }
            },
             makeToast(titulo, texto, tipo) {
                this.toastCount++
                this.$bvToast.toast(texto, {
                    title: titulo,
                    variant: tipo,
                    autoHideDelay: 3000,
                    appendToast: true
                })
            }
        },
        mounted() {
           this.getClients();
        },
    }
</script>

<style lang="">

</style>
