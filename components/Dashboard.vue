<template>
  <div>
    <div class="w-100 bg-gray text-center">
      Welcome {{ this.user.first_name }} {{ this.user.last_name }}
    </div>

    <div>
      <CreateProject
        v-bind:user="user"
        @saved="handleSaved"
        v-if="!showProjectKeys"
      />
      <CreateProjectKey
        v-else
        v-bind:user="user"
        v-bind:project="currentProject"
        @saved="handleSavedKey"
        v-on:back="handleBackToProjects"
      />
      <EditProject
        v-bind:project="currentProject"
        @updated="handleProjectUpdated"
        v-if="showEditProjectForm"
      />
      <EditProjectKey
        v-bind:projectKey="currentProjectKey"
        @updated="handleProjectKeyUpdated"
        v-if="showEditProjectKeyForm"
      />
    </div>

    <div>
      <Projects
        v-if="!showProjectKeys"
        v-bind:projects="userProjects"
        v-on:view="handleViewProject"
        v-on:deleted="handleProjectDeleted"
        v-on:update="handleUpdateProject"
      />
    </div>

    <div>
      <ProjectKeys
        v-if="showProjectKeys"
        v-bind:project="currentProject"
        v-bind:keys="currentProjectKeys"
        v-on:update="handleUpdateProjectKey"
        v-on:deleted="handleProjectKeyDeleted"
      />
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
      currentProject: {},
      currentProjectKeys: [],
      currentProjectKey: {},
      showProjectKeys: false,
      showEditProjectKeyForm: false,
      showEditProjectForm: false,
    }
  },
  methods: {
    handleViewProject(project) {
      this.currentProject = project

      this.showProjectKeys = true
      // Fetch the keys for the current project
      this.fetchProjectKeys()
    },
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
    handleUpdateProject(proj, idx) {
      this.currentProject = proj
      this.showEditProjectForm = true
    },
    handleProjectUpdated(proj) {
      const index = this.userProjects.findIndex((prj) => prj.id === proj.id)
      this.userProjects.splice(index, 1, proj)
      this.showEditProjectForm = false
    },

    fetchProjectKeys() {
      fetch(`http://localhost:3200/keys?project=${this.currentProject.id}`, {
        method: 'GET',
        headers: {
          'Content-Type': 'application/json',
          Authorization: `Bearer ${sessionStorage.getItem('authToken')}`,
        },
      })
        .then(async (res) => {
          const resp = await res.json()
          if (res.ok) {
            this.currentProjectKeys = resp
          }
        })
        .catch((err) => {
          console.log(err)
        })
    },
    handleSavedKey(data) {
      this.currentProjectKeys.push(data)
      this.fetchProjectKeys()
    },
    handleProjectKeyUpdated(key) {
      const index = this.currentProjectKeys.findIndex(
        (prjKey) => prjKey.id === key.id
      )
      this.currentProjectKeys.splice(index, 1, key)
      this.showEditProjectKeyForm = false
    },

    handleProjectKeyDeleted(id, idx) {
      this.currentProjectKeys.splice(idx, 1)
    },

    handleUpdateProjectKey(key, idx) {
      this.currentProjectKey = key
      this.showEditProjectKeyForm = true
    },
    handleBackToProjects() {
      this.showProjectKeys = false
      this.currentProject = {}
      this.currentProjectKeys = []
      this.currentProjectKey = {}
      this.showEditProjectKeyForm = false
    },
  },
}
</script>
