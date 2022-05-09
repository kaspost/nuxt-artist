<template>
  <div class="slug">
    <section class="img">
      <nuxt-img :src="post.featured_image" quality="80" format="webp" height="1900" />
    </section>
    <section class="content">
      <!-- <h1><TextTransition :text="post.title" :delay="0.2"></TextTransition></h1> -->
      <h1>{{ post.title }}</h1>
      <TextTransitionGroup :text="post.preamble"></TextTransitionGroup>
      <img :src="require(`~/assets/images/arrow.png`)" />
    </section>
  </div>
</template>

<style scoped>
  h1{
    margin-bottom: 35px;
  }
</style>

<script>
import TextTransition from '../components/TextTransition.vue';
import TextTransitionGroup from '../components/TextTransitionGroup.vue';
import gsap from 'gsap';

export default {
  components: { TextTransition, TextTransitionGroup },
  async asyncData({ params, $axios }) {
    const post = await $axios.$get(`http://nuxt.local/wp-json/nuxt-api/v1/post?slug=${params.slug}`);
    if (post.length === 0) throw({ statusCode: 404, message: 'Post not found' })
    return { post }
  },
  transition: {
    enter(el, done){
      const section = el.querySelector('.content')
      gsap.fromTo(section.children, { opacity: 0 }, { opacity: 1, ease: "Power3.easeIn", duration: 0.6, stagger: 0.2, onComplete: done })
    },
    leave(el){}
  }
}
</script>
