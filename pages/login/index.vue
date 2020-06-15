<template>
  <div class="wrapper">
    <form class="auth" @submit.prevent="login">
      <h2>LOGIN</h2>
      <input v-model="email" class="field" type="text" placeholder="Email" autocomplete="email">
      <span :class="{ error: true, 'error--active': emailError }">{{ emailError }}</span>
      <input
        v-model="password"
        class="field"
        type="password"
        placeholder="Password"
        autocomplete="password"
      >
      <span :class="{ error: true, 'error--active': passwordError }">{{ passwordError }}</span>
      <login-button type="submit" text="Login" />
    </form>
  </div>
</template>

<script>
import LoginButton from '~/components/LoginButton'

export default {
  components: {
    LoginButton
  },
  data () {
    return {
      email: '',
      password: '',
      emailError: null,
      passwordError: null
    }
  },
  methods: {
    async login () {
      this.emailError = null
      this.passwordError = null
      try {
        await this.$auth.loginWith('local', {
          data: {
            email: this.email.trim().toLowerCase(),
            password: this.password.trim()
          }
        })
      } catch (e) {
        // eslint-disable-next-line camelcase
        this.emailError = e?.response?.data?.first_errors?.email ?? null
        // eslint-disable-next-line camelcase
        this.passwordError = e?.response?.data?.first_errors?.password ?? null
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.wrapper {
  background-color: #f4f4f4;
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}
.auth {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: flex-start;
  width: 600px;
  min-height: 400px;
}
.field {
  min-width: 400px;
  background-color: #fff;
  border: 1px solid #cccccc;
  border-radius: 4px;
  padding: 16px 8px;
  font-size: 1rem;
  color: #151515;
  &::placeholder {
    color: #cccccc;
  }
}
.error {
  font-size: 1rem;
  color: #cc1f30;
  padding: 16px 0 16px 16px;
  &--active {
    border-left: 4px solid #cc1f30;
  }
}
</style>
