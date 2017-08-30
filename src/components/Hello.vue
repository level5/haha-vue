<template>

<v-layout row wrap>
  <v-flex xs12 sm6 offset-sm3>
    <v-card  :class="{gone: logined}">
      <v-card-title>
        <h5>Login</h5>
      </v-card-title>
      <v-card-text>
        <v-container>
          <v-layout row wrap>
            <v-flex sm6>
              <v-text-field
                    name="username"
                    label="username"
                    v-model="username"
                    light
                  ></v-text-field>
            </v-flex>
            <v-flex sm6>
              <v-text-field
                    name="input-1"
                    label="password"
                    type="password"
                    v-model="password"
                    light
                  ></v-text-field>
            </v-flex>
          </v-layout>
        </v-container>
      </v-card-text>
      <v-card-actions class="login-action">
        <v-btn primary right @click="submit">Login</v-btn>
      </v-card-actions>
    </v-card>
  </v-flex>
  <v-flex xs12 sm6 offset-sm3>
    <v-card  :class="{gone: !logined}">
      <v-card-title>
        <h5>Select</h5>
      </v-card-title>
      <v-card-text>
        <v-container>
          <v-layout row wrap>
            <v-flex sm6>
              <v-text-field
                    name="build"
                    label="build"
                    v-model="build"
                    light
                  ></v-text-field>
            </v-flex>
            <v-flex sm6>
              <v-text-field
                    name="input-1"
                    label="password"
                    type="password"
                    v-model="password"
                    light
                  ></v-text-field>
            </v-flex>
          </v-layout>
        </v-container>
      </v-card-text>
      <v-card-actions class="login-action">
        <v-btn primary right @click="submit">Login</v-btn>
      </v-card-actions>
    </v-card>
  </v-flex>
</v-layout>


</template>

<script>
import axios from 'axios'

var instance = axios.create({
  baseURL: 'https://rally1.rallydev.com/slm/webservice/v2.0/'
})

export default {
  name: 'hello',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      username: 'shifeng.huang@hpe.com',
      password: '!QAZ@WSX3edc',
      build: '',
      testSet: '',
      logined: false
    }
  },

  methods: {
    submit () {
      instance.get('/security/authorize', {
        headers: {'Authorization': 'Basic ' + btoa(this.username + ':' + this.password)}
      })
        .then(resp => {
          console.log(resp)
          this.logined = true

          instance.defaults.headers.common['Authorization'] =
            resp.data.OperationResult.SecurityToken
        })
        .then(() => {
          // instance.get()
        })
        .catch(err => {
          console.error(err)
          this.logined = false
        })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.login-action {
  justify-content: flex-end;
}

.gone {
  display: none;
}
</style>
