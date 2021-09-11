<template>
  <div>
    <div class="w-100 bg-gray text-center">
      Welcome {{ this.user.first_name }} {{ this.user.last_name }}
    </div>

    <div>
      <CreateProject v-bind:user="user" @saved="handleSaved" />
    </div>

    <div>
      <Projects
        v-bind:projects="userProjects"
        v-on:deleted="handleProjectDeleted"
      />
    </div>

    <div>
      <ProjectsKeys />
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    if (!sessionStorage.getItem('authToken')) {
      this.$nuxt.$options.router.push('/')
    }
    const u = sessionStorage.getItem('userData')
    if (u) {
      const user = JSON.parse(u)
      this.user = user
      this.accessToken = sessionStorage.getItem('authToken')
      this.fetchProjects()
    }
  },
  // An account is created for an organization/coy
  // Can have multiple projects
  // A project can have multiple keys
  data() {
    return {
      user: {},
      accessToken: '',
      userProjects: [],
    }
  },
  methods: {
    fetchProjects() {
      // Call the fetch projects endpoint from here

      fetch(`http://localhost:3200/projects?org=${this.user.org_id}`, {
        method: 'GET',
        headers: {
          'Content-Type': 'application/json',
          Authorization: `Bearer ${sessionStorage.getItem('authToken')}`,
        },
      })
        .then(async (res) => {
          const resp = await res.json()
          if (res.ok) {
            this.userProjects = resp
          }
        })
        .catch((err) => {
          console.log(err)
        })
    },
    handleSaved(data) {
      this.userProjects.push(data)
      this.fetchProjects()
    },
    handleProjectDeleted(id, idx) {
      this.userProjects.splice(idx, 1)
    },
  },
}
</script>
