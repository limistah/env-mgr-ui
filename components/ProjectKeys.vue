<template>
  <div class="container mb-2 mx-auto w-full">
    <div class="w-full m-3 text-center">Test Prj 1 Keys</div>
    <ul class="flex flex-col p-4">
      <li class="border-gray-400 flex flex-row">
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
            <div class="font-medium">Name</div>
          </div>

          <div class="flex-1 pl-1 mr-16">
            <div class="font-medium">Val</div>
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
            Edit
          </div>
          <div
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
