<template>
  <v-container>
    <h1>新規登録</h1>
    <v-text-field v-model="email" type="email" label="Email" />
    <v-text-field v-model="password" type="password" label="Password" />
    <v-text-field v-model="confirmPassword" type="password" label="confirmPassword" />
    <v-text-field v-model="name" type="text" label="Name" />

    <v-btn @click.prevent="signup">
      新規登録
    </v-btn>
    <div>{{ message }}</div>
    <div>
      <nuxt-link to="/login">
        ログインはこちら
      </nuxt-link>
    </div>
    <div>
      <nuxt-link to="/">
        一覧ページへ
      </nuxt-link>
    </div>
  </v-container>
</template>

<script>
import { createUserWithEmailAndPassword, updateProfile } from '@firebase/auth'
import { auth } from '../plugins/firebase'
export default {
  name: 'SignupPage',
  data () {
    return {
      email: '',
      password: '',
      confirmPassword: '',
      name: '',
      message: ''
    }
  },
  methods: {
    signup () {
      if (this.email.trim() && this.password.trim()) {
        if (this.password === this.confirmPassword) {
          createUserWithEmailAndPassword(auth, this.email, this.password)
            .then((userCredential) => {
              const user = userCredential.user
              updateProfile(user, { displayName: this.name })
                .then(() => {
                  const user = auth.currentUser
                  this.message = 'アカウントが作成されました: ' + (user.displayName || user.uid)
                  this.$router.push('/notice')
                })
            })
            .catch((err) => {
              this.message = err.message
            })
        } else {
          this.message = 'パスワードが一致しません'
        }
      }
    }
  }
}
</script>

<style>

</style>
