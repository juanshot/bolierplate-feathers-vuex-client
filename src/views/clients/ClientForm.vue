<template>
<v-flex xs12 sm10 offset-sm1>
    <v-card flat>
        <v-form v-model="valid">
            <v-text-field
            v-model="name"
            label="Nombre"
            required
            ></v-text-field>
            <v-text-field
            v-model="email"
            label="E-mail"
            required
            ></v-text-field>
            <v-btn flat color="blue" @click="enviarData">Guardar</v-btn>
        </v-form>
    </v-card>
</v-flex>
</template>

<script>
export default {
  props: ['limpiar'],
  data () {
    return {
      name: '',
      email: '',
      valid: false
    }
  },
  methods: {
    enviarData () {
      console.log({ name: this.name, email: this.email })
      this.$emit('guardado', { name: this.name, email: this.email })
    }
  },
  watch: {
    limpiar () {
      this.name = ''
      this.email = ''
    }
  }
}
</script>

<style>

</style>
