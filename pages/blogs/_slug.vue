<template>
  <section class="container">
    <div v-if="blog">
      <h1 class="title">
        {{ blog.fields.title }}
      </h1>
    </div>
  </section>
</template>

<script>
  import { createClient } from '~/plugins/contentful.js'
  import moment from 'moment'
  const client = createClient()

  export default {
    head() {
      return {
        title: this.blog ? this.blog.fields.title : '',
        meta: [
          {
            hid: 'description',
            name: 'description',
            content: this.blog ? this.blog.fields.description : '',
          }
        ]
      }
    },
    data() {
      return {
        blogData: '',
      }
    },
    computed: {
      blog () {
        return this.blogData
      },
    },
    beforeMount() {
      console.log(this.blog)
      this.getBlog()
    },
    methods: {
      getBlog() {
        return Promise.all([
          client.getEntries({
            'content_type': 'blogPost',
            'fields.slug': this.$route.params.slug,
          })
        ])
        .then(([blog]) => {
          this.blogData = blog.items[0] || ''
          return true
        })
        .catch((err) => {
          console.warn(err)
        })
      }
    },

    // validate ({ params }) {
    //   // Must be a number
    //   // TODO: Check param from contentful
    //   return /^\d+$/.test(params.slug)
    // }
  }
</script>