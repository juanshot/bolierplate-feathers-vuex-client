<template>
  <div class="home">
    <input @keyup.enter="newClient" v-model="text" type="text">
    <button @click="newClient">
      Crear Nuevo Cliente
    </button>
    <div v-for="client in clients" :key="client._id">
      {{client.text}}
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { mapGetters, mapActions } from 'vuex'

export default {
  name: 'home',
  data () {
    return {
      text: '',
      clients: []
    }
  },
  methods: {
    ...mapActions('clients', { getClientsFromService: 'find' }),
    newClient () {
      const { Client } = this.$FeathersVuex
      let cliente = new Client({ text: this.text })
      cliente.save().then((res) => {
        this.getClientsFromService().then(res => {
          this.text = ''
          this.clients = res.data
        })
      })
    }
  },
  computed: {
    ...mapGetters('clients', { getClientsInStore: 'find' }),
    getClients () {
      return this.getClientsInStore().data
    }
  },

  created () {
    console.log('este es el getter', this.getClients)
    this.getClientsFromService().then(res => {
      this.clients = res.data
    })
  },
  components: {
  }
}
</script>
