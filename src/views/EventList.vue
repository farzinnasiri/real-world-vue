<template>
  <div>
    <h1>Events for {{ user.user.name }}</h1>
    <EventCard v-for="event in event.events" :key="event.id" :event="event" />
    <template v-if="page != 1">
      <router-link
        :to="{ name: 'event-list', query: { page: page - 1 } }"
        rel="prev"
        >Prev Page</router-link
      >
      <template v-if="hasNextPage"> | </template>
    </template>
    <template v-if="hasNextPage">
      <router-link
        :to="{ name: 'event-list', query: { page: page + 1 } }"
        rel="next"
      >
        Next Page</router-link
      >
    </template>
  </div>
</template>
<script>
import EventCard from '@/components/EventCard.vue'
import { mapState } from 'vuex'

export default {
  components: {
    EventCard
  },
  created() {
    this.perPage = 4
    this.$store.dispatch('event/fetchEvents', {
      perPage: this.perPage, // <-- How many items to display per page
      page: this.page // <-- What page we're on
    })
    console.log(this.page)
  },
  computed: {
    page() {
      // What page we're currently on
      return parseInt(this.$route.query.page) || 1
    },
    hasNextPage() {
      return this.event.eventsTotal > this.page * this.perPage
    },
    ...mapState(['event', 'user'])
  }
}
</script>
