<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <h1>
        Students table task
      </h1>
    </v-app-bar>

    <v-main class="mt-10">
      <Main />
    </v-main>
  </v-app>
</template>

<script>
import Main from './components/Main'

export default {
  name: 'App',

  components: {
    Main
  }
}
</script>
