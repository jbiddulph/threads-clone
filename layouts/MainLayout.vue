<template>
  <div id="MainLayout" class="h-screen">
    <div class="w-full max-w-[500px] mx-auto">
      <div id="TopMenu" class="w-full flex items-center justify-center px-2 h-10 my-2 text-white">
        <img src="/threads-logo.png" class="w-[35px]" alt="">
      </div>
    </div>
    <div class="flex w-full max-w-[500px] mx-auto h-screen">
      <slot />
    </div>
    <div id="BottomNav" class="fixed z-50 bottom-0 flex w-full h-[70px] border-t border-t-gray-700 bg-black">
      <div class="flex w-full max-w-[500px] mx-auto items-center justify-around">
        <button @click="navigateTo('/')" class="w-full h-full">
          <Icon name="material-symbols:home-outline" size="35" color="#ffffff" style="color: white"/>
        </button>
        <button @click="userStore.isMenuOverlay = true" class="w-full h-full">
          <Icon name="material-symbols:edit-square-outline-rounded" size="35" color="#ffffff" style="color: white"/>
        </button>
        <button @click="userStore.isLogoutOverlay = true" class="w-full h-full">
          <Icon name="material-symbols:logout-rounded" size="35" color="#ffffff" style="color: white"/>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const userStore = useUserStore()
</script>
