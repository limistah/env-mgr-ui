<template>
  <div class="min-h-screen bg-gray-100 flex flex-col justify-center sm:py-12">
    <div class="p-10 xs:p-0 mx-auto md:w-full md:max-w-md">
      <h1 class="font-bold text-center text-2xl mb-5">Join Env Manager</h1>
      <div class="bg-white shadow w-full rounded-lg divide-y divide-gray-200">
        <div class="px-5 py-7">
          <label class="font-semibold text-sm text-gray-600 pb-1 block"
            >First Name</label
          >
          <input
            type="text"
            class="border rounded-lg px-3 py-2 mt-1 mb-5 text-sm w-full"
          />
          <label class="font-semibold text-sm text-gray-600 pb-1 block"
            >Last Name</label
          >
          <input
            type="text"
            class="border rounded-lg px-3 py-2 mt-1 mb-5 text-sm w-full"
          />
          <label class="font-semibold text-sm text-gray-600 pb-1 block"
            >E-mail</label
          >
          <input
            type="text"
            class="border rounded-lg px-3 py-2 mt-1 mb-5 text-sm w-full"
          />
          <label class="font-semibold text-sm text-gray-600 pb-1 block"
            >Password</label
          >
          <input
            type="text"
            class="border rounded-lg px-3 py-2 mt-1 mb-5 text-sm w-full"
          />
          <label class="font-semibold text-sm text-gray-600 pb-1 block"
            >Confirm Password</label
          >
          <input
            type="text"
            class="border rounded-lg px-3 py-2 mt-1 mb-5 text-sm w-full"
          />
          <button
            type="button"
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
            <span class="inline-block mr-2">Submit</span>
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
  </div>
</template>
