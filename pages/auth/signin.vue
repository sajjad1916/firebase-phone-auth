<template>
  <v-row align="center" justify="center">
    <v-col cols="12" sm="8" md="4" align="center">
      <v-card width="500" class="elevation-4 text-left" shaped color="yellow">
        <v-card-title>Login</v-card-title>
        <v-card-subtitle>Login to your dashboard</v-card-subtitle>
        <v-card-text>
          <v-form>
            <v-text-field
              label="Login"
              name="login"
              prepend-icon="mdi-account"
              type="text"
              v-model="auth.codes"
            ></v-text-field>

            <v-text-field
              label="Password"
              name="password"
              prepend-icon="mdi-lock"
              type="password"
              v-model="auth.password"
            ></v-text-field>
          </v-form>
        </v-card-text>
        <v-card-actions class="text-center">
          <v-btn
            class="login-button"
            id="recaptcha-container"
            @click="login"
            depressed
            large
            >Login</v-btn
          >
          <v-btn class="reset-button" @click="forgotPassword" depressed large
            >Forgot Password</v-btn
          >
        </v-card-actions>
      </v-card>
      <v-snackbar :timeout="4000" v-model="snackbar" absolute bottom center>
        {{ snackbarText }}
      </v-snackbar>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      snackbar: false,
      verificationId:'',
      snackbarText: 'No error message',
      auth: {
        codes: '',
        password: '',
      },
    }
  },
  mounted() {
    this.appVerifier = new this.$fireModule.auth.RecaptchaVerifier(
      'recaptcha-container',
      {
        size: 'invisible',
        callback: (response) => {
          console.log('workrs')
          // reCAPTCHA solved, allow signInWithPhoneNumber.
        },
      }
    )
  },
  methods: {
    async login() {
      const loginWithNumber = await this.$fire.auth.signInWithPhoneNumber(
        '+8801705768525',
        this.appVerifier
      )
      this.verificationId = loginWithNumber.verificationId;

      console.log(loginWithNumber);
      
    },
   async forgotPassword() {
    const result =  await this.$fireModule.auth.PhoneAuthProvider.credential(this.verificationId,this.auth.codes);
    console.log(result);
    },
  },
}
</script>

<style>
</style>