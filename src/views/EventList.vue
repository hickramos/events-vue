<template>
  <div>
    <h1>Events for {{ user.user.name }}</h1>

    <EventCard v-for="event in event.events" :key="event.id" :event="event" />

    <template v-if="showFirstPage">
      <router-link
        :to="{ name: 'event-list', query: { page: page - 1 } }"
        rel="prev"
      >
        Prev Page
      </router-link>
      |
    </template>
    <template v-if="showNextPage">
      <router-link
        :to="{ name: 'event-list', query: { page: page + 1 } }"
        rel="next"
      >
        Next Page
      </router-link>
    </template>
  </div>
</template>

<script>
import EventCard from '@/components/EventCard.vue';
import { mapState } from 'vuex';
import store from '@/store';

function getPageEvents(routeTo, next) {
  const currentPage = parseInt(routeTo.query.page) || 1;
  store
    .dispatch('event/fechEvents', {
      page: currentPage
    })
    .then(() => {
      routeTo.params.page = currentPage;
      next();
    });
}

export default {
  props: {
    page: {
      type: Number,
      required: true
    }
  },
  components: {
    EventCard
  },
  beforeRouteEnter(routeTo, routeFrom, next) {
    getPageEvents(routeTo, next);
  },
  beforeRouteUpdate(routeTo, routeFrom, next) {
    getPageEvents(routeTo, next);
  },
  created() {
    this.$store.dispatch('event/fechEvents', {
      perPage: 3,
      page: this.page
    });
  },
  computed: {
    showFirstPage() {
      return this.page != 1;
    },
    showNextPage() {
      return this.event.eventsTotal > this.page * this.event.perPage;
    },
    ...mapState(['event', 'eventsTotal', 'user'])
  }
};
</script>

<style></style>
