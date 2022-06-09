<template>
        <v-app-bar
      app
      color="purple"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />
      </div>
    </v-app-bar>
</template>

<script>
  export default {
    name: 'HeaderMain',
  };
</script>