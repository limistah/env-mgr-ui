<template>
  <div class="w-100 w-full p-5 b-1">
    <button
      class="
        border-2 border-indigo-600
        hover:bg-indigo-600 hover:color-white hover:ring-2
        rounded
        p-2
      "
      v-on:click="handleShowCreateProjectForm"
    >
      Create Project
    </button>
    <form
      v-if="showForm"
      class="bg-gray-100 flex flex-col justify-center py-4"
      v-on:submit.prevent="handleSubmit"
    >
      <div class="xs:p-0 mx-auto md:w-full md:max-w-md">
        <h1 class="font-bold text-center text-2xl mb-5">Create Project</h1>
        <div class="bg-white shadow w-full rounded-lg divide-y divide-gray-200">
          <div class="px-5 py-7">
            <label class="font-semibold text-sm text-gray-600 pb-1 block"
              >Name</label
            >
            <input
              v-model="name"
              type="text"
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
              <span class="inline-block mr-2">Create</span>
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
  </div>
</template>

<script>
export default {
  props: ['user'],
  data() {
    return {
      showForm: false,
      formMessage: '',
      loading: false,
      name: '',
    }
  },
  methods: {
    handleShowCreateProjectForm() {
      this.showForm = !this.showForm
    },
    handleSubmit() {
      if (this.error) {
        return
      }

      this.formMessage = ''
      // What is this?

      // Let's use fetch to push the details
      const body = {
        name: this.name,
        org_id: this.user.org_id,
      }

      this.loading = true

      fetch('http://localhost:3200/projects', {
        method: 'POST',
        body: JSON.stringify(body),
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
            this.formMessage = 'Project Created successfully.'
            this.$emit('saved', resp)
            this.showForm = false
          } else {
            const msg = resp.message
            this.formMessage = Array.isArray(msg) ? resp.message[0] : msg
          }
        })
        .catch((err) => {
          console.log(err)
          this.loading = false
          this.formMessage = 'An error occured. Please try again.'
        })
    },
  },
  computed: {
    error() {
      return !this.name || this.name.length < 3
        ? 'Please enter a valid name'
        : ''
    },
  },
}
</script>
