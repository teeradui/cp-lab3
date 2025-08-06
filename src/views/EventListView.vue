<script setup lang="ts">
import EventCard from '@/components/EventCard.vue'
import { type Event } from '@/types'
import { ref, onMounted, computed, watchEffect } from 'vue'
import EventService from '@/services/EventService'
import { useEventStore } from '@/stores/event'
import { storeToRefs } from 'pinia'

const store = useEventStore()
const { event } = storeToRefs(store)

const events = ref<Event[] | null>(null)
const totalEvents = ref(0)
const hasNextPage = computed(() => {
  const totalPages = Math.ceil(totalEvents.value / size.value)
  return page.value < totalPages
})
const props = defineProps({
  page: {
    type: Number,
    required: true
  },
  size: {
    type: Number,
    required: true
  }
})
const page = computed (() => props.page)
const size = computed(() => props.size)
onMounted(() => {
  watchEffect(() => {
    //events.value = null
    EventService.getEvents(size.value, page.value)
    .then((response) => {
      events.value = response.data
      totalEvents.value = response.headers['x-total-count']
    })
    .catch((error) => {
      console.error('There was an error!', error);
    })
  })
})
</script>

<template>
  <h1>Events For Good</h1>
  <!--new element-->
  <div class="flex flex-col items-center">
    <EventCard v-for="event in events" :key="event.id" :event="event" />

    <div class="flex w-[290px]">
      <RouterLink id="page-prev" class="flex-1 text-left text-[#2c3e50] no-underline" :to="{name: 'event-list-view', query: {page: page - 1, size: size }}" v-if="page > 1">&#60; Prev Page</RouterLink> 

      <RouterLink id="page-next" class="flex-1 text-right text-[#2c3e50] no-underline" :to="{name: 'event-list-view', query: {page: page + 1, size: size }}" v-if="hasNextPage">Next Page &#62;</RouterLink>
    </div>
  </div>
</template>


