<template>
  <v-app dark>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer
      app
      class="justify-center"
    >
      <span>Desenvolvido por <a href="https://www.linkedin.com/in/aleffsouza/">Aleff Souza</a> </span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data () {
    return {

    }
  }
}
</script>
