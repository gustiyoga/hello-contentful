<template>
  <section class="container">
    <div>
      <h1 class="title">
        Blogs
      </h1>
      <h2 class="subtitle">
        Blogs goes here
      </h2>
    </div>
    <br><br>

    <div class="columns is-multiline">
      <div v-show="isLoading" class="column is-one-quarter" v-for="index in 4" :key="index">
        <content-loader
          :height="475"
          :width="403"
          :speed="1"
          primaryColor="#f3f3f3"
          secondaryColor="#ecebeb"
        >
          <rect x="10" y="20" rx="0" ry="0" width="340.93" height="227" /> 
          <rect x="75" y="262" rx="1" ry="1" width="274.73" height="38.7" /> 
          <circle cx="38.84874347350331" cy="289.1187434735033" r="28.848743473503312" /> 
          <rect x="75" y="310.27" rx="0" ry="0" width="159" height="22.96" /> 
          <rect x="16" y="349.27" rx="0" ry="0" width="335" height="27.01" /> 
          <rect x="16" y="384.27" rx="0" ry="0" width="335" height="27.01" /> 
          <rect x="16" y="420.27" rx="0" ry="0" width="335" height="27.01" />
        </content-loader>
      </div>

      <div v-if="blogs === 0 && isLoading === false">
        <h3> Tidak ada data </h3>
      </div>
      <div
        v-for="blog in blogs"
        :key="blog.index"
        class="column is-one-quarter">      
        <card
          :data="blog"
          :index="blog.index"></card>
      </div>
    </div>
  </section>
</template>

<script>
  import {createClient} from '~/plugins/contentful.js'
  import { ContentLoader } from 'vue-content-loader'
  import Card from '~/components/Card.vue'
  const client = createClient()

  export default {
    components: {
      ContentLoader,
      Card,
    },
    data() {
      return {
        blogsData: 0,
        isLoading: true,
      }
    },
    computed: {
      blogs () {
        return this.blogsData
      },
    },
    beforeMount() {
      this.getBlogs()
    },
    methods: {
      getBlogs() {
        this.isLoading = true

        return Promise.all([
          client.getEntries({
            'content_type': 'blogPost',
            order: '-sys.createdAt',
            // limit: 1,
          })
        ])
        .then(([events]) => {
          this.blogsData = events.items || 0
          this.isLoading = false
          return true
        })
        .catch((err) => {
          this.isLoading = false
          console.warn(err)
        })
      },
    },
  }
</script>
