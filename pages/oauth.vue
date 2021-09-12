<template>
  <div>
    <Login
      v-on:login="handleLoggedIn"
      v-bind:auth="true"
      v-bind:title="loginTitle"
    />
  </div>
</template>

<script>
export default {
  mounted() {
    // Check if there is authentication token
    // Call the auth endpoint to generate a 2days token
    // Call the callback url with the auth token
    // if there is none, show the login form
  },
  data() {
    return {
      loginTitle: 'Authorize Onboard',
    }
  },
  methods: {
    handleLoggedIn() {
      this.fetchLongLiveToken()
    },
    fetchLongLiveToken() {},
  },
}
</script>
