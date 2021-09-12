<template>
  <div class="container">
    <!-- <h1 class="text-5xl text-center p-3 bg-gray-200">Onboard Env Manager</h1> -->
    <Login v-on:login="handleLoggedIn" v-bind:title="loginTitle" />
  </div>
</template>

<script lang="ts">
export default {
  data() {
    return {
      loginTitle: 'Login to Onboard',
    }
  },
  methods: {
    handleLoggedIn() {
      this.$nuxt.$options.router.push('/dashboard')
    },
  },
}
</script>
