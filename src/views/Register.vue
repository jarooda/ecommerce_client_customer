<template>
  <div class="container-fluid pt-5">
    <div class="row pt-5">
      <div class="col-sm-4"></div>
      <div class="col-sm-4 text-center border shadow-lg pb-3">
        <h1 class="text-success pt-2">Register</h1>
        <form @submit.prevent="register" class="pt-3">
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text bg-success" id="basic-addon1"><i class="fas fa-envelope text-white"></i></span>
            </div>
            <input type="email" class="form-control" placeholder="Email" aria-label="Email" aria-describedby="basic-addon1"
            v-model="email">
          </div>
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text bg-success" id="basic-addon1"><i class="fas fa-lock text-white"></i></span>
            </div>
            <input type="password" class="form-control" placeholder="Password" aria-label="Password" aria-describedby="basic-addon1"
            v-model="password">
          </div>

          <input type="submit" class="btn btn-success" value="Register">
        </form>
        <hr>
        Login Using <br>
        <GoogleLogin :params="params" class="btn btn-success mt-1" :onSuccess="onSuccess"><i class="fab fa-google text-white"></i></GoogleLogin>
      </div>
      <div class="col-sm-4"></div>
    </div>
  </div>
</template>

<script>
import axios from '../config/axios'
import GoogleLogin from 'vue-google-login'
export default {
  name: 'Register',
  data () {
    return {
      email: '',
      password: '',
      // client_id is the only required property but you can add several more params, full list down bellow on the Auth api section
      params: {
        client_id: '1067495165024-hh81b6hotacbg58epk8kfk4ahg9oa09e.apps.googleusercontent.com'
      }
    }
  },
  components: {
    GoogleLogin
  },
  methods: {
    register () {
      axios({
        url: '/register',
        method: 'post',
        data: {
          email: this.email,
          password: this.password
        }
      }).then(({ data }) => {
        this.$swal({
          icon: 'success',
          title: 'Success Created an Account',
          showConfirmButton: false,
          timer: 1500
        })
        this.$router.push('/login').catch(() => { })
      }).catch(err => {
        this.$swal({
          icon: 'error',
          title: 'Oops...',
          text: err.response.data.message
        })
      })
    },
    onSuccess (googleUser) {
      const token = googleUser.getAuthResponse().id_token
      this.$store.dispatch('GOOGLELOGIN', { token })
    }
  }
}
</script>
