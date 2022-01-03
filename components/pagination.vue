<template>
  <section class="pagination py-3 my-2 flex justify-between text-white">
    <a
      title="Previous Page"
      class="cursor-pointer"
      :class="{ disabled: page == 1 }"
      @click.prevent="previousPage()"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        style="width: 32px"
        fill="none"
        viewBox="0 0 24 24"
        stroke="#fff"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M11 19l-7-7 7-7m8 14l-7-7 7-7"
        />
      </svg>
    </a>
    <div>Page {{ page }}</div>
    <a
      title="Next Page"
      class="cursor-pointer"
      :class="{ disabled: page == maxPage }"
      @click.prevent="nextPage()"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        style="width: 32px"
        fill="none"
        viewBox="0 0 24 24"
        stroke="#fff"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M13 5l7 7-7 7M5 5l7 7-7 7"
        />
      </svg>
    </a>
  </section>
</template>
<script>
export default {
  props: {
    page: {
      type: Number,
      default: 1,
    },
    maxPage: {
      type: Number,
      default: 50,
    },
  },
  methods: {
    nextPage() {
      this.$emit('nextPage')
    },
    previousPage() {
      this.$emit('previousPage')
    },
  },
}
</script>
