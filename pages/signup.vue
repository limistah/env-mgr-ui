<template>
  <div class="container">
    <Signup />
  </div>
</template>

<script lang="ts">
export default {
  mounted() {
    if (sessionStorage.getItem('authToken')) {
      location.replace('/dashboard')
    }
  },
}
</script>
