<template>
  <div>
    <v-card class="pa-5">
      <v-card-text>
        <v-row class="mx-auto">
          <v-spacer></v-spacer>
          <v-btn text @click="logout_FNC">logout</v-btn>
        </v-row>
        <v-card-title>Hello <span class="font-weight-medium mx-2">{{userName}}</span>, ready for the day task?
        </v-card-title>
      </v-card-text>

    </v-card>
    <v-card class="mt-2 pa-5">
      <!-- todo app component -->
      <TodoCom />
    </v-card>
  </div>
</template>

<script>
import TodoCom from '~/components/todoCom.vue';
export default {
  components: { TodoCom },
  middleware: "auth",
  data: () => ({}),
  computed: {
    userName() {
      return this.$auth.user.name;
    }
  },
  created() {

  },
  methods: {

    logout_FNC() {
      this.$auth.logout();
      this.$router.push("/login");
    },


  },

}
</script>

<style>

</style>