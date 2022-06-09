<template>
    <v-footer padless>
      <v-col
        class="text-center"
        cols="12"
      >
        {{ new Date().getFullYear() }} — <strong>Devoted Studios</strong>
      </v-col>
    </v-footer>
</template>

<script>
  export default {
    name: 'FooterMain',
  };
</script>