<template>
  <div>
    <Login
      v-on:login="handleLoggedIn"
      v-bind:auth="true"
      v-bind:title="loginTitle"
      v-if="!hasToken"
    />
    <div v-if="authorizing">
      {{ authorizationMessage }}
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    if (sessionStorage.getItem('authToken') && !this.auth) {
      this.hasToken = true
    }
    this.authorizing = true
    this.authorizationMessage = 'Authorizing'
    this.fetchLongLiveToken()
    // Check if there is authentication token
    // Call the auth endpoint to generate a 2days token
    // Call the callback url with the auth token
    // if there is none, show the login form
  },
  data() {
    return {
      authorizationMessage: '',
      hasToken: false,
      loading: false,
      success: false,
      authorizing: false,
      loginTitle: 'Authorize Onboard',
    }
  },
  methods: {
    handleLoggedIn() {
      this.hasToken = true
      this.authorizing = true
      this.authorizationMessage = 'Authorizing'
      this.fetchLongLiveToken()
    },
    fetchLongLiveToken() {
      fetch('https://env-mgr.herokuapp.com/auth/oauth', {
        headers: {
          'Content-Type': 'application/json',
          Authorization: `Bearer ${sessionStorage.getItem('authToken')}`,
        },
      })
        .then(async (res) => {
          const resp = await res.json()
          this.loading = false
          if (res.ok) {
            this.success = true
            // this.clearForm()
            this.authorizationMessage =
              'Authorization Successful. Continue in your terminal'
            const query = `&token=${resp.access_token}&user=${JSON.stringify(
              resp.user
            )}&project=${this.$route.query.project}&org_id=${resp.user.org_id}`
            const fullURL = this.$route.query.callback + query
            fetch(fullURL).then((res) => {
              if (res.ok) {
                console.log('Successful')
              }
            })
            // sessionStorage.setItem('authToken', resp.access_token)
            // sessionStorage.setItem('userData', JSON.stringify(resp.user))
            // this.$emit('login', resp)
          } else {
            const msg = resp.message
            this.authorizationMessage = Array.isArray(msg)
              ? resp.message[0]
              : msg
          }
        })
        .catch((err) => {
          this.loading = false
          this.authorizationMessage = 'An error occured. Please try again.'
        })
    },
  },
}
</script>
