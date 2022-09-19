<template>
  <v-app dark>
    <div class="sibtitle pa-5">Todo App</div>
  
  
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
  
    <v-footer :absolute="!fixed" app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  auth:false,
  name: 'DefaultLayout',
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Home',
          to: '/'
        },
        {
          icon: 'mdi-account',
          title: 'Login',
          to: '/'
        },

      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'todo App'
    }
  },
  methods:{
   
  }
}
</script>
