<template>
  <v-app>
    <HeaderMain />

    <v-main>
      <MyTable/>
    </v-main>

    <FooterMain />
    
  </v-app>
</template>

<script>
import MyTable from './components/MyTable';
import FooterMain from './components/FooterMain';
import HeaderMain from './components/HeaderMain';


export default {
  name: 'App',

  components: {
    MyTable,
    FooterMain,
    HeaderMain,
  },

  data: () => ({
    //
  }),
};
</script>