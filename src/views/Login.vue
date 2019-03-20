<template>
  <div class="auth-page">
    <div class="container page">
      <div class="row">
        <div class="col-md-6 offset-md-3 col-xs-12">
          <h1 class="text-xs-center">Sign in</h1>
          <p class="text-xs-center">
            <router-link :to="{ name: 'register' }">
              Need an account?
            </router-link>
          </p>
          <ul v-if="errors" class="error-messages">
            <li v-for="(v, k) in errors" :key="k">{{ k }} {{ v | error }}</li>
          </ul>
          <form id="myForm" v-on:submit.prevent="onSubmit(email, password, castle_client_id);">
            <fieldset class="form-group">
              <input id="email"
                class="form-control form-control-lg"
                type="text"
                v-model="email"
                placeholder="Email"
              />
            </fieldset>
            <fieldset class="form-group">
              <input id="password"
                class="form-control form-control-lg"
                type="password"
                v-model="password"
                placeholder="Password"
              />
            </fieldset>
            <fieldset class="form-group">
              <input id="castle"
                class="form-control form-control-lg"
                type="text"
                v-model="castle_client_id"
                placeholder="don't input here it will automatically be done with castle"
              />
            </fieldset>
            <button class="btn btn-lg btn-primary pull-xs-right">
              Sign in
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";
import { LOGIN } from "@/store/actions.type";



export default {
    mounted() {
    let recaptchaScript = document.createElement('script')
    // castle CDN -- put your app id after the js? to replace YOUR-APP-ID
    recaptchaScript.setAttribute('src', 'https://d2t77mnxyo7adj.cloudfront.net/v1/c.js?YOUR-APP-ID')
    document.head.appendChild(recaptchaScript)
    // _castle('identify', 'e325bcdd10ac') 
    var clientId = _castle('getClientId');
    console.log(clientId)
    document.getElementById('castle').value = clientId
    document.getElementById('email').value = "hey@hey.com"
    document.getElementById('password').value = "ascend1008"
  },
  name: "RwvLogin",
  data() {
    return {
      email: null,
      password: null
    };
  },
  methods: {
    onSubmit(email, password, castle_client_id) {
      console.log(email, password, castle_client_id)
      this.$store
        .dispatch(LOGIN, { email, password, castle_client_id })
        .then(() => this.$router.push({ name: "home" }));
    }
  },
  computed: {
    ...mapState({
      errors: state => state.auth.errors
    })
  }
};
</script>
