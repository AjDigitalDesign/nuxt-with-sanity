<template>
  <div class="container">
    <section class="header-area mt-lg-5 mb-lg-4">
      <div class="header-area align-items-center d-flex flex-sm-column flex-lg-row justify-content-between">
        <h1 class="display-1 fw-bold">Blog</h1>
        <h5 class="flex-lg-shrink-1 text-lg-end ps-lg-5">Home Builder LLC. is a Washington, DC based home builder focused on building personalized, cutting-edge homes.</h5>
      </div>
    </section>

    <section class="latest-post">
        <div v-for="fp in featuredPost"  :key="fp._id" class="featured_post">
          <div class="featured-img">
            <img
              :src="$urlFor(fp.imageUrl)"
              loading="lazy"
              alt=""
              class="img-fluid"
            >
          </div>
          <div class="featured_post-content">
            <div class="featured_post-title">
              <h5><NuxtLink :to="fp.slug.current">{{fp.title}}</NuxtLink></h5>
              <span class="data">{{formatDate(fp.publishedAt)}}</span>
            </div>
            <div class="featured_post_excerpt">
                <p>{{fp.except}}</p>
            </div>
          </div>
      </div>
    </section>

    <section class="posts pt-5">
        <div class="container">
            <div class="row">
                <div v-for="post in posts" :key="post._id"  class="col-sm-12 col-md-6 col-lg-4">
                    <div class="post">
                      <div class="post-img">
                        <img
                          :src="$urlFor(post.imageUrl)"
                          loading="lazy"
                          alt=""
                          class="img-fluid"
                        >
                      </div>
                      <div class="post-content">
                        <h5><NuxtLink :to="post.slug.current">{{post.title}}</NuxtLink></h5>
                        <span class="data">{{formatDate(post.publishedAt)}}</span>
                        <div class="featured_post_excerpt">
                          <p>{{post.except}}</p>
                        </div>
                      </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

  </div>
</template>

<script>
import {groq} from '@nuxtjs/sanity'
export default {
  name: 'IndexPage',
  async  asyncData({$sanity}){
    const query =  groq `*[_type =='post']{
      "imageUrl": mainImage.asset->url,
      "except" : body[0].children[0].text,
      ...
    }`

    const posts =  await $sanity.fetch(query)
    const featuredPost = posts.slice(0,1)
    const allPost = posts.shift();
    return {posts, featuredPost, allPost}

  },
  methods: {
    formatDate(date){
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('en-us', options)
    }
  },
}
</script>
