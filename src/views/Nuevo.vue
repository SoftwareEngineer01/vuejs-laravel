<template>
    <div>
        <Header />

        <div class="container my-1 left">
            <h2 class="my-2 ">Agregar Cliente</h2>

            <form action="">
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
                    <button type="button" class="btn btn-primary mx-1" v-on:click="save()">Registrar</button>
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
        name: 'Nuevo',
        components: {
            Header,
            Footer
        },
         data() {
            return {
                form: {
                    'name': '',
                    'surname': '',
                    'identification': '',
                    'telephone': ''
                }
            }
        },
        methods: {
            save() {
                let url = 'http://localhost:8100/api/cliente';
                axios
                    .post(url, this.form)
                    .then( response => {
                        if(response.status == 200) {
                            this. makeToast('Realizado', 'Cliente registrado', 'success'),
                            setTimeout( () => {
                                this.$router.push('/dashboard');
                            }, 3000)
                        } else {
                            this. makeToast('Error', 'Error al guardar registro', 'danger');
                        }
                    })
                    .catch(error => {
                        console.log(error),
                        this. makeToast('Error', 'Error al guardar registro', 'danger');
                    })
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
    }
</script>

<style scoped>
    .left {
        text-align: left;
    }
</style>
