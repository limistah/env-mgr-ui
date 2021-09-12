<template>
  <form
    class="min-h-screen bg-gray-100 flex flex-col justify-center sm:py-12"
    v-on:submit.prevent="handleSubmit"
  >
    <div class="p-10 xs:p-0 mx-auto md:w-full md:max-w-md">
      <h1 class="font-bold text-center text-2xl mb-5">{{ title }}</h1>
      <div class="bg-white shadow w-full rounded-lg divide-y divide-gray-200">
        <div class="px-5 py-7">
          <label class="font-semibold text-sm text-gray-600 pb-1 block"
            >E-mail</label
          >
          <input
            v-model="email"
            type="email"
            class="border rounded-lg px-3 py-2 mt-1 mb-5 text-sm w-full"
          />
          <label class="font-semibold text-sm text-gray-600 pb-1 block"
            >Password</label
          >
          <input
            v-model="password"
            type="password"
            class="border rounded-lg px-3 py-2 mt-1 mb-5 text-sm w-full"
          />
          <div class="w-full">
            {{ error }}
          </div>

          <div class="w-full">
            {{ formMessage }}
          </div>

          <button
            :disabled="loading"
            type="submit"
            class="
              transition
              duration-200
              bg-blue-500
              hover:bg-blue-600
              focus:bg-blue-700
              focus:shadow-sm
              focus:ring-4
              focus:ring-blue-500
              focus:ring-opacity-50
              text-white
              w-full
              py-2.5
              rounded-lg
              text-sm
              shadow-sm
              hover:shadow-md
              font-semibold
              text-center
              inline-block
            "
          >
            <span class="inline-block mr-2">Login</span>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              class="w-4 h-4 inline-block"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M17 8l4 4m0 0l-4 4m4-4H3"
              />
            </svg>
          </button>
        </div>
      </div>
    </div>
  </form>
</template>

<script>
export default {
  props: ['auth', 'title'],
  mounted() {
    if (sessionStorage.getItem('authToken') && !this.auth) {
      this.$nuxt.$options.router.push('/dashboard')
    }
  },
  data() {
    return {
      password: '',
      email: '',
      loading: false,
      formMessage: '',
      success: false,
    }
  },
  methods: {
    handleSubmit() {
      if (this.error) {
        return
      }
      const body = {
        firstName: this.firstName,
        lastName: this.lastName,
        email: this.email,
        password: this.password,
      }

      this.loading = true

      fetch('http://localhost:3200/auth/login', {
        method: 'POST',
        body: JSON.stringify(body),
        headers: {
          'Content-Type': 'application/json',
        },
      })
        .then(async (res) => {
          const resp = await res.json()
          this.loading = false
          if (res.ok) {
            this.success = true
            // this.clearForm()
            this.formMessage = 'Login Successful'
            sessionStorage.setItem('authToken', resp.access_token)
            sessionStorage.setItem('userData', JSON.stringify(resp.user))
            this.$emit('login', resp)
          } else {
            const msg = resp.message
            this.formMessage = Array.isArray(msg) ? resp.message[0] : msg
          }
        })
        .catch((err) => {
          this.loading = false
          this.success = false
          this.formMessage = 'An error occured. Please try again.'
        })
    },
    clearForm() {
      this.email = ''
      this.password = ''
    },
  },
  computed: {
    error() {
      if (!this.success) {
        if (!this.email) {
          return 'Email is required'
        }

        if (!this.password) {
          return 'Password is required'
        }
      }

      return ''
    },
  },
}
</script>
