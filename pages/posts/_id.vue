<template>
  <section class="post">
    <article class="container">
      <div class="post-content">
        <div v-html="postContent" />
        <Share :url="url" />
        <disqus ref="disqus" v-bind:shortname="disqusShortname" :identifier="disqusId" />
      </div>
    </article>
  </section>
</template>

<style>
.post {
  font-family: Nunito,Helvetica Neue,Helvetica,Arial,sans-serif;
  font-weight: 400;
  line-height: 1.62;
  color: #444;
  -webkit-font-smoothing: antialiased;
  background: #fff;
  border-radius: 4px;
  box-shadow: 0 1px 2px rgba(0,0,0,0.07);
  margin-bottom: 5rem;
  margin-left: 0;
  margin-right: 0;
}

.post-content {
  margin: 1rem 0;
  background: #fff;
}

.post-content h1 {
  margin: 2rem 0;
  padding-top: 2rem;
  line-height: 2rem;
  font-size: 1.6rem;
}

.post-content pre {
  display: block;
  overflow-x: auto;
  color: #abb2bf;
  background: #282c34;
  width: 100%;
  padding: 1rem 1rem;
  margin: 48px 0;
  margin: 3rem -1rem;
  width: calc(100% + 2rem);
  font-size: .9rem;
}

.post-content p > code {
  color: #c7254e;
  background-color: #f9f2f4;
  border-radius: 4px;
  padding: 2px 4px;
  font-size: 90%;
}

.post-content img {
  width: 100%;
}

.post-content h2,
.post-content h3,
.post-content h4 {
  margin-bottom: 2rem;
}

.post p {
  margin-bottom: 3rem;
  font-size: 1.15rem;
}

.post-content a {
  text-decoration: underline;
}

.post-content blockquote {
  background: #f5f5f5;
  padding: 1rem;
  margin-bottom: 1rem;
  border: 1px solid #ccc;
}

.post-content blockquote p {
  margin-bottom: 0.03rem
}

@media screen and (min-width: 734px) {
  .post-content pre {
    margin-left: -2rem;
    margin-right: -2rem;
    box-sizing: content-box;
  }
  .post-content,
  .post-content pre {
    padding: 2rem;
  }
  .post-content h1 {
    font-size: 2rem;
  }
  .post-content img {
    width: auto;
    max-width: 100%;
  }
}

@media screen and (min-width: 1024px) {
  .post-content,
  .post-content pre, {
    padding: 2rem 4rem;
  }
}
</style>

<script type="text/babel">
import Disqus from 'vue-disqus/VueDisqus.vue'
import Share from '~/components/Share.vue'
import { posts } from '~/assets/posts.json'

export default {
  head () {
    return {
      title: this.post.title
    }
  },
  components: {
    Disqus,
    Share
  },
  computed: {
    postContent () {
      const { id } = this.$route.params
      return require(`~/assets/posts/${id}.md`)
    },
    disqusShortname () {
      return 'bloglucasmaiaesilva'
    },
    disqusId () { // env used to avoid re-use from dev to production
      return `${process.env.NODE_ENV}-${this.disqusShortname}-${this.$route.params.id}`
    },
    title () {
      return this.$route.params.id
    },
    post () {
      return posts.find(post => post.slug === this.title)
    },
    url () {
      return `http://lucasmaiaesilva.com.br/${this.title}`
    }
  },
  watch: {
    '$route.params.id' (curr, old) {
      // disqus does not properly reload just based off the
      // disqusId computed property - we need to manually change it
      // when we know it should update
      this.$refs.disqus.init()
    }
  }
}

</script>
