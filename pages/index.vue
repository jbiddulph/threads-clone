<template>
  <MainLayout>
    <div id="IndexPage" class="w-full overflow-auto">
      <div class="mx-auto max-w-[500px] overflow-hidden">
        <div v-if="isPosts" id="Posts" class="px-4 max-w-[600px] mx-auto">
          <div v-for="post in posts" :key="post.name">
            <Post :post="post" @isDeleted="posts = userStore.getAllPosts()" />
          </div>
        </div>
        <div v-else>
          <client-only>
            <div v-if="isLoading" class="mt-20 w-full flex items-center justify-center  mx-auto">
              <div class="text-white mx-auto flex flex-col items-center justify-center">
                <Icon name="eos-icons:bubble-loading" color="white" size="50" />
                <div class="w-full mt-1">Loading...</div>
              </div>
            </div>
            <div v-if="!isLoading" class="mt-20 w-full flex items-center justify-center  mx-auto">
              <div class="text-white mx-auto flex flex-col items-center justify-center">
                <Icon name="tabler:mood-empty" color="white" size="50" />
                <div class="w-full mt-1">Make the first post!</div>
              </div>
            </div>
          </client-only>
        </div>
      </div>
    </div>
  </MainLayout>
</template>

<script setup lang="ts">
import { ref, watchEffect, onBeforeMount } from 'vue'
import { useRouter } from 'vue-router'
import MainLayout from "../layouts/MainLayout.vue"

const userStore = useUserStore()
const user = useSupabaseUser()
const router = useRouter()

let posts = ref([])
let isPosts = ref(false)
let isLoading = ref(false)

watchEffect(() => {
  if (!user.value) {
    // Avoid multiple redirects by checking the current route
    if (router.currentRoute.value.path !== '/') {
      router.push('/')
    }
  }
})

onBeforeMount(async () => {
  try {
    isLoading.value = true
    await userStore.getAllPosts()
    isLoading.value = false
  } catch (error) {
    console.log(error)
  }
})

onMounted(() => {
  watchEffect(() => {
    posts.value = userStore.posts
    if (userStore.posts && userStore.posts.length >= 1) {
      isPosts.value = true
    } else {
      isPosts.value = false
    }
  })
})

watch(() => posts.value, () => {
  posts.value = userStore.posts
  if (userStore.posts && userStore.posts.length >= 1) {
    isPosts.value = true
  } else {
    isPosts.value = false
  }
}, { deep: true })
</script>
