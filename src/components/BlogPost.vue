<script>
  import { butter } from '@/buttercms'
  export default {
    name: 'blog-home',
    data() {
      return {
        page_title: 'Blog',
        posts: []
      }
    },
    methods: {
       fetchPosts () {
        butter.post.list({ page: 1, pageSize: 10 })
          .then((response) => {
            this.loading = false
            this.posts = response.data
          }).catch((response) => {
            console.log(response)
          })
      },
    },
    watch: {
      $route: {
        immediate: true,
        handler(to, from) {
          this.fetchPosts()
        }
      }
    },
    created() {
      this.fetchPosts()
    }
  }
</script>

<template>
  <div id="blog-post">
    <h1>{{ post.data.title }}</h1>
    <h4>{{ post.data.author.first_name }} {{ post.data.author.last_name }}</h4>
    <div v-html="post.data.body"></div>

    <router-link
      v-if="post.meta.previous_post"
      :to="/blog/ + post.meta.previous_post.slug"
      class="button"
    >
      {{ post.meta.previous_post.title }}
    </router-link>
    <router-link
      v-if="post.meta.next_post"
      :to="/blog/ + post.meta.next_post.slug"
      class="button"
    >
      {{ post.meta.next_post.title }}
    </router-link>
  </div>
</template>