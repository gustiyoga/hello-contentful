<template>
  <div
    class="card"
    @click="goTo(blog.fields.slug)">
      <div class="card-image">
      <figure class="image is-4by3">
          <content-loader
            :height="202"
            :width="270"
            :speed="1"
            v-show="!isAuthorImageLoaded"
            primaryColor="#f3f3f3"
            secondaryColor="#ecebeb"
            style="margin-top: -203px; position: absolute;">
            <rect x="0" y="0" rx="5" ry="5" width="372" height="220.00000000000003" />
          </content-loader>
          <img
            v-show="isCardImageLoaded"
            :src="blog.fields.heroImage.fields.file.url + '?w=640&h=480'"
            @load="imageLoader('CARD')"
            class="js-card-image"
            alt="Placeholder image">
      </figure>
    </div>
    <div class="card-content">
      <div class="media">
        <div class="media-left">
          <figure class="image is-48x48">
            <content-loader
              :height="60"
              :width="60"
              :speed="1"
              v-show="!isAuthorImageLoaded"
              primaryColor="#f3f3f3"
              secondaryColor="#ecebeb">
              <circle cx="30" cy="30" r="30" />
            </content-loader>
            <img
              v-show="isAuthorImageLoaded"
              :src="blog.fields.author.fields.image.fields.file.url + '?w=48&h=48&fit=fill'"
              :alt="blog.fields.author.fields.image.fields.description"
              @load="imageLoader('AUTHOR')"
              class="is-rounded js-author-image">
          </figure>
        </div>
        <div class="media-content">
          <p class="title is-5">{{ blog.fields.title }}</p>
          <p class="subtitle is-6">{{ blog.fields.author.fields.email }}</p>
        </div>
      </div>

      <div class="content">
        {{ subStrings(blog.fields.description) }}
        <br>
        <div>{{ dateConverter(blog.sys.createdAt) }}</div>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .card {
    cursor: pointer;
  }
  .is-rounded {
    box-shadow: 0 2px 14px rgba(10, 10, 10, 0.5);
  }
</style>

<script>
  import { ContentLoader } from 'vue-content-loader'
  import moment from 'moment'

  export default {
    name: 'Card',
    components: {
      ContentLoader,
    },
    data() {
      return {
        isAuthorImageLoaded: false,
        isCardImageLoaded: false,
        blog: this.data
      }
    },
    props: ['data'],
    mounted() {
      this.imageChecker()
    },
    methods: {
      goTo(id) {
        console.log(id)
        this.$router.push({
          name: 'blogs-slug',
          params:{slug:id},
        })
        return true
      },
      dateConverter(d) {
        return moment(d).format('h:mm a - DD MMM YYYY')
      },
      subStrings(str) {
        return str.substring(0, 100);
      },
      imageLoader(toggleData) {
        console.log('loaded: ' + toggleData)
        switch (toggleData) {
          case 'CARD':
            this.isCardImageLoaded = true
            break;
          case 'AUTHOR':
            this.isAuthorImageLoaded = true
          default:
            break;
        }
      },
      imageChecker() {
        let image = document.getElementsByClassName('js-card-image')
        console.log(image)
        if(image[0].complete) {
          console.log('loaded')
        }else{
          console.log('error')
        }
      }
    }
  }
</script>