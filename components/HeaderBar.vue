<template>
  <div>
    <nav class="nav">
      <h1>Q</h1>
      <div class="nav-left">
        <span class="nav-item">{{ title }}</span>
        <button type="button" class="nav-button nav-item" @click="logout">
          Logout
        </button>
      </div>
    </nav>
    <snackbar :message="message" :error="error" />
  </div>
</template>

<script>
import Snackbar from '~/components/Snackbar'

function getErrorText (error) {
  // eslint-disable-next-line camelcase
  const firstErrors = error?.response?.data?.first_errors ?? null
  const messages = Object.values(firstErrors)
  if (messages.length > 0) {
    return messages[0]
  }
  return 'Some error occurred'
}

export default {
  name: 'HeaderBar',
  components: {
    Snackbar
  },
  data () {
    return {
      error: false,
      message: null
    }
  },
  props: {
    title: {
      type: String,
      required: false,
      default: ''
    }
  },
  methods: {
    resetSnack () {
      setTimeout(() => {
        this.error = false
        this.message = null
      }, 3000)
    },
    async logout () {
      try {
        await this.$auth.logout()
        this.$router.push({ path: '/login' })
      } catch (e) {
        this.error = true
        this.message = getErrorText(e)
        this.resetSnack()
      }
    }
  }
}
</script>

<style lang="scss" scoped>
$nav-height: 64px;

.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background-color: #fafafa;
  width: 100vw;
  height: $nav-height;
  border-bottom: 1px solid rgba(0,0,0,0.5);
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  box-shadow: -1px 4px 8px 0px rgba(0,0,0,0.5);

  &-left {
    display: flex;
  }

  &-item {
    font-size: 1.2rem;
    color: #7b7888;
    margin-right: 16px;
    padding: 16px;
  }

  &-button {
    border: none;
    background-color: transparent;

    &:hover {
      cursor: pointer;
    }
  }
}
</style>
