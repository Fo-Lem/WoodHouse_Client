<script lang="ts">
import { defineComponent } from 'vue'
import { isAdmin } from './adminComponents/http/isAdmin'
import { relogin } from './adminComponents/http/relogin'

interface State {
  isAdminAuth: boolean
  mobileMenuOpen: boolean
}
export default defineComponent({
  name: 'CustomAdminPanelWrapper',
  props: {
    catalog: {
      type: Object,
      require: true,
    },

  },

  emits: ['updateData'],

  data(): State {
    return {

      mobileMenuOpen: false,
      isAdminAuth: false,

    }
  },
  async beforeMount() {
    if (await isAdmin()) {
      this.isAdminAuth = true
    }
    else {
      if (await relogin())
        this.isAdminAuth = true

      else
        this.isAdminAuth = false
    }
  },

})
</script>

<template>
  <div>
    <div>
      <button
        v-if="isAdminAuth"
        data-drawer-target="default-sidebar"
        data-drawer-toggle="default-sidebar"
        aria-controls="default-sidebar"
        type="button"
        class="inline-flex items-center p-2 mt-2 ml-3 text-sm text-gray-500 rounded-lg sm:hidden hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-200 dark:text-gray-400 dark:hover:bg-gray-700 dark:focus:ring-gray-600"
        @click="mobileMenuOpen = true"
      >
        <span class="sr-only">Open sidebar</span>
        <svg
          class="w-6 h-6"
          aria-hidden="true"
          fill="currentColor"
          viewBox="0 0 20 20"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            clip-rule="evenodd"
            fill-rule="evenodd"
            d="M2 4.75A.75.75 0 012.75 4h14.5a.75.75 0 010 1.5H2.75A.75.75 0 012 4.75zm0 10.5a.75.75 0 01.75-.75h7.5a.75.75 0 010 1.5h-7.5a.75.75 0 01-.75-.75zM2 10a.75.75 0 01.75-.75h14.5a.75.75 0 010 1.5H2.75A.75.75 0 012 10z"
          />
        </svg>
      </button>

      <aside
        v-if="isAdminAuth"
        id="default-sidebar"
        class="fixed top-0 left-0 z-40 w-64 h-screen transition-transform -translate-x-full sm:translate-x-0"
        :class="{ 'translate-x-0': mobileMenuOpen }"
        aria-label="Sidebar"
      >
        <div class="h-full px-3 py-4 overflow-y-auto bg-gray-50 dark:bg-gray-800">
          <ul class="space-y-2 font-medium">
            <li>
              <router-link
                :to="{ name: 'products' }"
                class="flex items-center p-2 text-gray-900 rounded-lg dark:text-white hover:bg-gray-100 dark:hover:bg-gray-700"
                @click="mobileMenuOpen = false"
              >
                <svg
                  aria-hidden="true"
                  class="flex-shrink-0 w-6 h-6 text-gray-500 transition duration-75 dark:text-gray-400 group-hover:text-gray-900 dark:group-hover:text-white"
                  fill="currentColor"
                  viewBox="0 0 20 20"
                  xmlns="http://www.w3.org/2000/svg"
                ><path
                  fill-rule="evenodd"
                  d="M10 2a4 4 0 00-4 4v1H5a1 1 0 00-.994.89l-1 9A1 1 0 004 18h12a1 1 0 00.994-1.11l-1-9A1 1 0 0015 7h-1V6a4 4 0 00-4-4zm2 5V6a2 2 0 10-4 0v1h4zm-6 3a1 1 0 112 0 1 1 0 01-2 0zm7-1a1 1 0 100 2 1 1 0 000-2z"
                  clip-rule="evenodd"
                /></svg>
                <span class="flex-1 ml-3 whitespace-nowrap">Продукты</span>
              </router-link>
            </li>
            <li>
              <router-link
                :to="{ name: 'panelCreations' }"
                class="flex items-center p-2 text-gray-900 rounded-lg dark:text-white hover:bg-gray-100 dark:hover:bg-gray-700"
                @click="mobileMenuOpen = false"
              >
                <svg
                  aria-hidden="true"
                  class="flex-shrink-0 w-6 h-6 text-gray-500 transition duration-75 dark:text-gray-400 group-hover:text-gray-900 dark:group-hover:text-white"
                  fill="currentColor"
                  viewBox="0 0 20 20"
                  xmlns="http://www.w3.org/2000/svg"
                ><path d="M8.707 7.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l2-2a1 1 0 00-1.414-1.414L11 7.586V3a1 1 0 10-2 0v4.586l-.293-.293z" /><path d="M3 5a2 2 0 012-2h1a1 1 0 010 2H5v7h2l1 2h4l1-2h2V5h-1a1 1 0 110-2h1a2 2 0 012 2v10a2 2 0 01-2 2H5a2 2 0 01-2-2V5z" /></svg>
                <span class="flex-1 ml-3 whitespace-nowrap">Добавить продукт</span>
              </router-link>
            </li>
          </ul>
        </div>
      </aside>

      <div class="p-4 " :class="{ 'sm:ml-64': isAdminAuth }">
        <router-view
          :is-admin-auth="isAdminAuth"
          :catalog="catalog"
          @is-admin-auth="isAdminAuth = true"
          @update-data="$emit('updateData')"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
.router-link-active{
   border: 1px solid gray;
}
</style>
