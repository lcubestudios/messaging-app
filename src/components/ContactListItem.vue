<template>
  <div class="flex flex-row flex-nowrap w-full bg-background h-80px border-b border-gray-200 cursor-pointer">
    <div class="w-full flex items-center justify-between px-4">
      <div class="relative" @click="goToMessages(item.username)">
        <div class="profile-icon bg-primary"></div>
        <span 
          v-if="item.unread && item.unread !== '0'"
          class="absolute top-0 right-0 flex justify-center items-center w-5 h-5 rounded-full text-xs bg-alert text-white"
        >
          {{ item.unread }}
        </span>
      </div>
      <div class="flex-1 text-left px-4 select-none" @click="goToMessages(item.username)">
        {{ item.username }}
      </div>
      <div v-if="!isToolbarVisible" class="flex items-center justify-center text-primary">
        <button class="relative btn-icon" @click="showToolbar">
          <mdicon name="dots-vertical" />
        </button>
      </div>
    </div>
    <div v-if="isToolbarVisible" class="flex items-center justify-center">
      <button
        class="relative btn-icon bg-alert text-white flex items-center justify-center w-80px h-80px"
        @click="deleteContact(item.username)"
        v-click-outside="hideToolbar"
      >
        <mdicon name="delete" />
      </button>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
import { useStore } from 'vuex'
import vClickOutside from 'click-outside-vue3'

export default {
  name: 'ContactListItem',
  directives: {
    clickOutside: vClickOutside.directive,
  },
  props: {
    item: {
      type: [Object],
    },
  },
  setup() {
    const store = useStore()
    const isToolbarVisible = ref(false)
    const showToolbar = () => {
      isToolbarVisible.value = true
    }
    const hideToolbar = () => {
      isToolbarVisible.value = false
    }
    const goToMessages = (username) => {
      store.dispatch('setView', 'messages')
      store.dispatch('setCurrContact', username)
      store.dispatch('getMessages', username)
    }
    const deleteContact = (username) => {
      store.dispatch('deleteContact', username)
    }

    return {
      isToolbarVisible,
      showToolbar,
      hideToolbar,
      goToMessages,
      deleteContact,
    }
  },
}
</script>