<template>
  <div>
    <section class="bg-gray-700 p-2 md:p-5 w-full">
      <form
        class="w-full md:w-3/4 xl:w-1/2 flex items-center justify-center m-auto"
        @submit.prevent="Search()"
      >
        <input
          id="input"
          ref="input"
          v-model="q"
          type="text"
          placeholder="Search keywords..."
          aria-label="Image Search input"
          class="p-2 w-6/12 rounded-l-md rounded-r-none leading-tight appearance-none focus:bg-gray-200 focus:border-gray-500 focus:outline-none"
          @click="$refs.input.select()"
        />
        <select
          v-model="image_type"
          class="p-2 w-3/12 leading-tight appearance-none bg-gray-100 focus:bg-gray-200 focus:border-gray-500 focus:outline-none"
        >
          <option value="all">All</option>
          <option value="photo">Photo</option>
          <option value="illustration">Illustration</option>
          <option value="vector">Vector</option>
        </select>
        <button
          id="submit"
          class="p-2 w-3/12 bg-yellow-500 rounded-r-md leading-tight appearance-none text-md text-white hover:bg-yellow-600 focus:outline-none"
          type="submit"
        >
          Search
        </button>
      </form>
    </section>

    <div class="px-3 h-fit">
      <pagination
        :page="page"
        :max-page="max_page"
        @previousPage="previousPage()"
        @nextPage="nextPage()"
      />
      <section
        id="photos"
        ref="imageElement"
        class="p-4 bg-gray-800 grid grid-cols-1 xl:grid-cols-5 lg:grid-cols-4 md:grid-cols-3 sm:grid-cols-2 gap-4 mx-auto min-h-screen"
      >
        <a
          v-for="image in images"
          :key="image.id"
          target="_blank"
          :href="image.largeImageURL"
          class="shadow-sm inline-block border w-full h-60 border-gray-600 overflow-hidden"
          ><img
            class="transform hover:scale-105 transition object-cover w-full h-full duration-100 rounded-sm"
            :alt="image.tags[0]"
            :src="image.webformatURL"
        /></a>
      </section>
      <pagination
        :page="page"
        :max-page="max_page"
        @previousPage="previousPage()"
        @nextPage="nextPage()"
      />
    </div>
  </div>
</template>
<script setup>
import Pagination from '~/components/pagination.vue'

export default {
  data() {
    return {
      images: [],
      q: 'sky',
      image_type: 'all',
      api_key: '25025948-0104efb8ef62a18e144720da8',
      page: 1,
      per_page: 40,
      max_page: 50,
      imgLoaded: 0,
    }
  },
  async fetch() {
    await this.getImages()
  },
  fetchOnServer: false,
  methods: {
    nextPage() {
      const p =
        parseInt(this.page) < this.max_page
          ? parseInt(this.page) + 1
          : this.page
      this.page = p
      this.$fetch()
    },
    previousPage() {
      const p = parseInt(this.page) > 1 ? parseInt(this.page) - 1 : this.page
      this.page = p
      this.$fetch()
    },
    getImages() {
      this.$axios
        .get(
          `/api/?key=${this.api_key}&q=${this.q}&image_type=${this.image_type}&per_page=${this.per_page}&page=${this.page}`
        )
        .then((response) => {
          this.images = response.data.hits
          this.max_page = Math.ceil(response.data.totalHits / this.per_page) - 1
        })
    },
    Search() {
      this.page = 1
      this.$fetch()
    },
  },
}
</script>

Pagination
