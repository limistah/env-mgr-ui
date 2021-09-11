<template>
  <div class="container mb-2 mx-auto w-full">
    <div class="w-full m-3 text-center">All Projects</div>
    <div class="w-full m-3 text-center" v-if="deleteMessage">
      {{ deleteMessage }}
    </div>
    <ul class="flex flex-col p-4">
      <li
        class="border-gray-400 flex flex-row"
        v-for="(item, index) in projects"
        :key="item.id"
      >
        <div
          class="
            select-none
            flex flex-1
            items-center
            p-4
            transition
            duration-500
            ease-in-out
            transform
            hover:-translate-y-2
            rounded-2xl
            border-2
            p-6
            hover:shadow-2xl
            border-red-400
          "
        >
          <div class="flex-1 pl-1 mr-16">
            <div class="font-medium">{{ item.name }}</div>
          </div>
          <div
            class="
              w-1/6
              text-wrap text-center
              flex
              text-white text-bold
              flex-col
              rounded-md
              bg-blue-500
              justify-center
              items-center
              mr-10
              p-2
              cursor-pointer
            "
          >
            View
          </div>
          <div
            class="
              w-1/6
              text-wrap text-center
              flex
              text-white text-bold
              flex-col
              rounded-md
              bg-blue-500
              justify-center
              items-center
              mr-10
              p-2
              cursor-pointer
            "
            v-on:click="handleEditItem(item, index)"
          >
            Edit
          </div>
          <div
            v-on:click="handleDeleteItem(item.id, index)"
            class="
              w-1/6
              text-wrap text-center
              flex
              text-white text-bold
              flex-col
              rounded-md
              bg-red-500
              justify-center
              items-center
              mr-10
              p-2
              cursor-pointer
            "
          >
            Delete
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ['projects'],
  data() {
    return {
      deleteMessage: '',
    }
  },
  methods: {
    handleDeleteItem(id, idx) {
      fetch(`http://localhost:3200/projects/${id}`, {
        method: 'DELETE',
        headers: {
          'Content-Type': 'application/json',
          Authorization: `Bearer ${sessionStorage.getItem('authToken')}`,
        },
      })
        .then(async (res) => {
          const resp = await res.json()
          if (res.ok) {
            this.$emit('deleted', id, idx)
          } else {
            const msg = resp.message
            this.deleteMessage = Array.isArray(msg) ? resp.message[0] : msg
          }
        })
        .catch((err) => {
          this.deleteMessage = err.message
          console.log(err)
        })
    },
    handleEditItem(item, idx) {
      this.$emit('update', item, idx)
    },
  },
  computed: {},
}
</script>
