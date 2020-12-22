<template>
    <div>
        <Header />

        <div class="container my-1 left">
            <h2 class="my-2">Editar Cliente</h2>

            <form action="" class="my-3">
                <div class="form-row">
                    <div class="form-group col-md-8">
                        <label for="name">Nombre</label>
                        <input type="text" class="form-control" id="name" v-model="form.name">
                    </div>
                    <div class="form-group col-md-8">
                        <label for="surname">Apellido</label>
                        <input type="text" class="form-control" id="surname" v-model="form.surname">
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group col-md-8">
                        <label for="identification">Identificación</label>
                        <input type="text" class="form-control" id="identification" v-model="form.identification">
                    </div>
                    <div class="form-group col-md-8">
                        <label for="telephone">Teléfono</label>
                        <input type="text" class="form-control" id="telephone" v-model="form.telephone">
                    </div>
                </div>
                <div class="form-group my-1">
                    <button type="button" class="btn btn-primary mx-1" v-on:click="update()">Actualizar</button>
                    <button type="button" class="btn btn-warning mx-1" v-on:click="cancel()">Cancelar</button>
                </div>
            </form>

        </div>

        <Footer />
    </div>
</template>

<script>
    import Header from '@/components/Header.vue';
    import Footer from '@/components/Footer.vue';
    import axios from 'axios';

    export default {
        name: "Editar",
        components: {
            Header,
            Footer
        },
        data() {
            return {
                clienteId:  this.$route.params.id,
                form: {
                    'name': '',
                    'surname': '',
                    'identification': '',
                    'telephone': ''
                }
            }
        },
        methods: {
            getClientId() {
                 axios
                .get("http://localhost:8100/api/cliente/" + this.clienteId)
                .then( data => {
                    let result = data.data.data;
                    this.form.name = result.name;
                    this.form.surname = result.surname;
                    this.form.identification = result.identification;
                    this.form.telephone = result.telephone;
                })
                .catch(error => console.log(error))
            },
            update() {
                let url = 'http://localhost:8100/api/cliente/'+this.clienteId;
                axios
                    .put(url, this.form)
                    .then( response => {
                        if (response.status == 200) {
                            this. makeToast('Realizado', 'Cliente actualizado', 'success'),
                            setTimeout( () => {
                                this.$router.push("/dashboard");
                            },3000)
                        } else {
                            console.log('Ha ocurrido un error al actualizar el registro');
                        }

                    })
                    .catch(error => console.log(error))
            },
            cancel() {
                this.$router.push("/dashboard");
            },
            makeToast(titulo, texto, tipo) {
                this.toastCount++
                this.$bvToast.toast(texto, {
                    title: titulo,
                    variant: tipo,
                    autoHideDelay: 2000,
                    appendToast: true
                })
            }
        },
        mounted() {
           this.getClientId();
        },
    }
</script>

<style scoped>
    .left {
        text-align: left;
    }
</style>
