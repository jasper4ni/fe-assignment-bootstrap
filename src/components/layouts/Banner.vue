<template>
  <div class="py-6 banner">
    <div class="container h-full flex items-center flex-wrap sm:flex-nowrap">
      <img src="/images/Avatar.png" class="h-36 w-36" />
      <div class="flex flex-col p-6 text-white min-w-max">
        <h2 class="text-4xl font-semibold mb-1">John Doe</h2>
        <p class="text-sm font-thin">Last online: 2 days ago</p>
      </div>
      <div class="flex items-center flex-wrap sm:flex-nowrap mt-3 sm:mt-0">
        <RouterLink to="/">
          <button
            class="flex items-center justify-center text-cyan-500 border-2 border-white bg-white sm:w-44 py-3 px-4 mr-3 rounded"
          >
            <mdicon name="send" size="20" class="mr-1 inline-block" />
            <p class="font-semibold">Send Message</p>
          </button>
        </RouterLink>

        <RouterLink to="/">
          <button
            class="flex items-center justify-center bg-transparent border-white border-2 text-white sm:w-44 py-3 px-4 rounded"
          >
            <mdicon name="plus" size="20" class="mr-1 inline-block" />
            <p class="font-semibold">Add Friend</p>
          </button>
        </RouterLink>
      </div>
    </div>
  </div>
</template>
