<template>
  <button class="button" v-bind="$attrs" v-on="$listeners">
    {{ text }}
  </button>
</template>

<script>
export default {
  name: 'LoginButton',
  props: {
    text: {
      type: String,
      required: true
    }
  }
}
</script>

<style lang="scss" scoped>
.button {
  background-color: #395378;
  border: 1px solid #395378;
  border-radius: 8px;
  padding: 16px;
  font-size: 1rem;
  font-weight: bold;
  color: #fff;
  text-transform: uppercase;
  cursor: pointer;
}
</style>
