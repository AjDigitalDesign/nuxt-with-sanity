<template>
  <div>
    <section class="blog-post pt-5">
      <div class="container">
        <div class="row">
          <div v-if="post" class="col-lg-12">
            <h1>{{post.title}}</h1>
            <img
              :src="$urlFor(post.imageUrl)"
              loading="lazy"
              alt=""
              class="img-fluid"
            >
            <div class="content pt-4">
              <SanityContent :blocks="post.body" />
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
  name: "PostSlug",


  async asyncData({params, $sanity}){
    const query =  groq `*[_type =='post' && slug.current == "${params.slug}"][0]{
      "imageUrl": mainImage.asset->url,
      "except" : body[0].children[0].text,
      ...
    }`
    const post = await $sanity.fetch(query);
    return {post}
  }
}
</script>

<style scoped>

</style>
