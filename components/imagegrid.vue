<template>
  <div>
    <section class="bg-gray-700 p-5 w-full">
      <form
        class="w-full md:w-3/4 xl:w-1/2 flex items-center justify-center m-auto"
        @submit.prevent="Search()"
      >
        <input
          id="input"
          v-model="q"
          type="text"
          placeholder="Search keywords..."
          aria-label="Image Search input"
          class="p-2 w-8/12 rounded-l-md leading-tight appearance-none focus:bg-gray-200 focus:border-gray-500"
        />
        <select
          v-model="image_type"
          class="p-2 w-2/12 leading-tight appearance-none bg-gray-100 focus:bg-gray-200 focus:border-gray-500"
        >
          <option value="all">All</option>
          <option value="photo">Photo</option>
          <option value="illustration">Illustration</option>
          <option value="vector">Vector</option>
        </select>
        <button
          id="submit"
          class="p-2 w-2/12 bg-yellow-500 rounded-r-md leading-tight appearance-none text-md text-white hover:bg-yellow-600"
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
        class="p-4 bg-gray-800 grid grid-cols-1 xl:grid-cols-5 lg:grid-cols-4 md:grid-cols-3 sm:grid-cols-2 gap-4 mx-auto npm min-h-screen"
      >
        <a
          v-for="image in images"
          :key="image.id"
          target="_blank"
          :href="image.largeImageURL"
          class="shadow-sm inline-block md:max-h-44 max-h-96"
          :class="{ hidden: !showImgGrid }"
          ><img
            class="object-cover border border-gray-600 transform hover:scale-105 transition duration-100 rounded-sm w-full h-full"
            :alt="image.tags[0]"
            :src="image.webformatURL"
            @load="handleLoad"
        /></a>
        <spinner :class="{ hidden: showImgGrid }"></spinner>
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
import Spinner from '~/components/spinner.vue'

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
      showImgGrid: false,
    }
  },
  mounted() {
    this.getImages()
  },
  methods: {
    nextPage() {
      const p =
        parseInt(this.page) < this.max_page
          ? parseInt(this.page) + 1
          : this.page
      this.page = p
      this.getImages()
    },
    previousPage() {
      const p = parseInt(this.page) > 1 ? parseInt(this.page) - 1 : this.page
      this.page = p
      this.getImages()
    },
    getImages() {
      this.showImgGrid = false
      this.$axios
        .get(
          `https://pixabay.com/api?key=${this.api_key}&q=${this.q}&image_type=${this.image_type}&per_page=${this.per_page}&page=${this.page}`
        )
        .then((response) => {
          this.images = response.data.hits
          this.max_page = Math.ceil(response.data.totalHits / this.per_page)
        })
    },
    Search() {
      this.page = 1
      this.getImages()
    },
    handleLoad() {
      this.imgLoaded++
      if (this.imgLoaded === this.per_page) {
        this.showImgGrid = true
        this.imgLoaded = 0
      }
    },
  },
}
</script>

Pagination
