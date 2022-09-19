<template>
  <div>
    <div>


      <v-card class="pa-5 mt-10">
        <div class="text-center mb-5">{{title}}</div>
        <v-text-field type="" dense label="user email" v-model="email"></v-text-field>
        <v-text-field type="" class="mt-5" dense v-model="pass" label="user password"></v-text-field>
        <v-row class="mx-auto mt-5">
          <v-btn @click.prevent="login_FNC" v-if="loginShow" :loading="login_BTN_LDR">Login</v-btn>
          <v-btn @click.prevent="register_FNC" v-if="registerShow">Register</v-btn>
          <v-spacer></v-spacer>
          <v-btn text small @click="registerShow_FNC" v-if="regBtnShow">Register</v-btn>
          <v-btn text small @click="loginShow_FNC" v-if="loginBtnShow">Login</v-btn>

        </v-row>
      </v-card>
    </div>

  </div>
</template>

<script>
export default {
  auth: 'guest',
  data: () => ({
    login_BTN_LDR:false,
    loginShow: true,
    registerShow: false,
    regBtnShow: true,
    loginBtnShow: false,
    title: 'Login',
    email: 'test@gmail.com',
    pass: 'udit123'
  }),
  mounted() {
    this.$axios.$get('/sanctum/csrf-cookie');
  },
  methods: {
    async login_FNC() {
      try {
        this.login_BTN_LDR = true;
        this.registerShow = false
        let pl = {
          email: this.email,
          password: this.pass
        }

        await this.$auth.loginWith('laravelSanctum', { data: pl });
        console.log('login')
        this.$router.push({
          path: '/',
        });
      } catch (error) {
        this.login_BTN_LDR = true;
        console.log(error)
      }
    },
    async register_FNC() {
      try {

        this.loginShow = false
        let pl = {
          name: 'demo',
          email: this.email,
          password: this.pass
        }

        await this.$axios.post('/register', pl);
        console.log('register')
        this.$router.push({
          path: '/',
        });
      } catch (error) {
        console.log(error)
      }
    },
    registerShow_FNC() {
      this.title = "Register"
      this.registerShow = true
      this.loginShow = false
      this.regBtnShow = false
      this.loginBtnShow = true
    },
    loginShow_FNC() {
      this.title = "Login"
      this.loginShow = true
      this.registerShow = false
      this.loginBtnShow = false
      this.regBtnShow = true
    }
  }
}
</script>

<style>

</style>