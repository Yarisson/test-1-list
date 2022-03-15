<template>
  <div
    class="item"
    :class="{ active: hover }"
    @mouseover="hover = true"
    @mouseleave="hover = false">
    <div class="item__left">
      <span v-if="match" class="item__match">
        <svg
          width="24"
          height="24"
          stroke-width="1.5"
          viewBox="0 0 24 24"
          fill="none"
          xmlns="http://www.w3.org/2000/svg">
          <path
            d="M1.5 12.5L5.57574 16.5757C5.81005 16.8101 6.18995 16.8101 6.42426 16.5757L9 14"
            stroke="currentColor"
            stroke-linecap="round" />
          <path d="M16 7L12 11" stroke="currentColor" stroke-linecap="round" />
          <path
            d="M7 12L11.5757 16.5757C11.8101 16.8101 12.1899 16.8101 12.4243 16.5757L22 7"
            stroke="currentColor"
            stroke-linecap="round" />
        </svg>
      </span>
      <div class="item__name-wrapper">
        <span class="item__name">{{ itemElement.name }}</span>
        <div>
          <span v-if="match" class="item__match-text">exact match</span>
          <span class="item__number">#{{ itemElement.number }}</span>
        </div>
      </div>
    </div>
    <div class="item__right">
      <span class="item__time">{{ timeDistance }} ago</span>
      <button v-show="hover" class="item__basket" @click="deleteElement">
        <svg
          width="24"
          height="24"
          stroke-width="1.5"
          viewBox="0 0 24 24"
          fill="none"
          xmlns="http://www.w3.org/2000/svg">
          <path
            d="M19 11V20.4C19 20.7314 18.7314 21 18.4 21H5.6C5.26863 21 5 20.7314 5 20.4V11"
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round" />
          <path
            d="M10 17V11"
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round" />
          <path
            d="M14 17V11"
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round" />
          <path
            d="M21 7L16 7M3 7L8 7M8 7V3.6C8 3.26863 8.26863 3 8.6 3L15.4 3C15.7314 3 16 3.26863 16 3.6V7M8 7L16 7"
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round" />
        </svg>
      </button>
    </div>
  </div>
</template>
<script>
import { formatDistance, parseISO } from 'date-fns';

export default {
  name: 'Item',
  components: {},
  props: ['itemElement', 'match'],
  data: () => ({
    hover: false,
  }),
  computed: {
    timeDistance() {
      return formatDistance(new Date(), parseISO(this.itemElement.time));
    },
  },
  methods: {
    deleteElement() {
      this.$emit('deleteUser', this.itemElement.number);
    },
  },
};
</script>

<style lang="scss">
@import '../sass/Item.scss';
</style>
