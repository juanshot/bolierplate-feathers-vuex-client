<template>
 <v-layout>
    <v-flex xs12 sm10 offset-sm1 class="mt-5">
      <v-card>
        <v-card-title primary-title>
          <div>
            <h3 class="headline mb-0">Formulario</h3>
          </div>
        </v-card-title>
        <client-form :limpiar="limpiar" @guardado="llamarApi"></client-form>
      </v-card>
      <v-flex v-for="client in clients" :key="client._id" xs12 class="mt-2">
        <v-card dark color="light-blue">
          <v-card-text>{{client.name}}</v-card-text>
        </v-card>
      </v-flex>
    </v-flex>
  </v-layout>
</template>

<script>
import { mapActions } from 'vuex'
import ClientForm from './ClientForm'
export default {
  data () {
    return {
      limpiar: false,
      clients: []
    }
  },
  methods: {
    ...mapActions('clients', { findFormService: 'find' }),
    llamarApi (data) {
      console.log('desde el padre obtengo esta data', data)
      const { Client } = this.$FeathersVuex
      let client = new Client(data)
      client.save().then((res) => {
        this.findFormService({ query: { $limit: null } }).then(res => {
          this.clients = res.data
          console.log('esto recibo', this.clients)
          this.limpiarCampos()
        })
      })
    },
    limpiarCampos () {
      this.limpiar = !this.limpiar
    }
  },
  created () {
    this.findFormService({ query: { $limit: null, name: 'jose' } }).then(res => {
      this.clients = res.data
      console.log('esto recibo', this.clients)
    })
  },
  components: { ClientForm }
}
</script>

<style>

</style>
