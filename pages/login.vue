<template>
  <v-app>
    <v-container>
      <h1>ログイン</h1>
      <v-text-field v-model="email" type="email" label="Email" />
      <v-text-field v-model="password" type="password" label="Password" />
      <v-btn @click.prevent="login">
        ログイン
      </v-btn>
      <div>{{ message }}</div>
      <div>
        <nuxt-link to="/signup">
          新規アカウント作成はこちら
        </nuxt-link>
      </div>
      <div>
        <nuxt-link to="/">
          一覧ページへ
        </nuxt-link>
      </div>
    </v-container>
  </v-app>
</template>

<script>
import { signInWithEmailAndPassword } from '@firebase/auth'
import { auth } from '../plugins/firebase'

export default {
  name: 'LoginPage',
  data () {
    return {
      email: '',
      password: '',
      message: ''
    }
  },
  methods: {
    login () {
      if (this.email.trim() && this.password.trim()) {
        signInWithEmailAndPassword(auth, this.email, this.password)
          .then(() => {
            this.$router.push('/')
            this.message = 'Login! : '
          })
          .catch(() => {
            this.message = 'ログインできません。'
          })
      }
    }
  }
}
</script>
