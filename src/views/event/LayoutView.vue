<script setup lang="ts">
import { useEventStore } from '@/stores/event'
import { storeToRefs } from 'pinia'
/*import { ref, onMounted } from 'vue'
import { type Event } from '@/types'
import EventService from '@/services/EventService'
import { useRouter } from 'vue-router'


const event = ref<Event | null>(null)
const props = defineProps({
  id: {
    type: String,
    required: true,
  },
})
const router = useRouter()*/
const store = useEventStore()
const { event } = storeToRefs(store)

/*onMounted(() => {
  EventService.getEvent(parseInt(props.id))
    .then((response) => {
      event.value = response.data
    })
    .catch((error) => {
      console.log('error ', error)
      if (error.response && error.response.status === 404) {
        router.push({
          name: '404-resource-view',
          params: { resource: 'event' },
        })
      } else {
        router.push({ name: 'network-error-view' })
      }
    })
})*/
</script>

<template>
  <div v-if="event">
    <h1>{{ event.title }}</h1>
    <nav>
      <router-link :to="{ name: 'event-detail-view' }">Details</router-link> |
      <router-link :to="{ name: 'event-register-view' }">Register</router-link> |
      <router-link :to="{ name: 'event-edit-view' }">Edit</router-link>
    </nav>
    <RouterView :event="event" />
  </div>
</template>
