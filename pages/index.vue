<template>
  <div class="hero">
    <section>
      <div>
        <img :src="require(`~/assets/images/1.png`)" />
        <h2>01</h2>
      </div>
    </section>
    <section @click="click" v-on="!clicked ? { mouseover: imgHoverIn, mouseout:  imgHoverOut } : {}">
      <div>
        <div>
          <div>
            <img :src="require(`~/assets/images/2.png`)" />
            <div>
              <p>Follow</p>
              <p>your</p>
              <p>dreams</p>
            </div>
          </div>
        </div>
        <h2>02</h2>
      </div>
      <div class="hero-post">
        <nuxt-img :src="post.hero_post.featured_image" quality="80" format="webp" height="1900" />
      </div>
    </section>
    <section>
      <div>
        <div>
          <img :src="require(`~/assets/images/3.png`)" />
          <div>
            <p>Lorem ipsum Lorem ipsum Lorem ipsum Lorem ipsum Lorem ipsum Lorem ipsumLorem ipsum Lorem ipsum Lorem ipsum Lorem ipsum Lorem ipsum Lorem ipsum Lorem ipsum </p>
            <img :src="require(`~/assets/images/arrow.png`)" />
          </div>
        </div>
        <h2>03</h2>
      </div>
    </section>
    <section>
      <div>
        <img :src="require(`~/assets/images/4.png`)" />
        <h2>04</h2>
      </div>
    </section>
  </div>
</template>

<style scoped>
.hero{
  display: none;
}
</style>

<script>
import gsap from "gsap";

export default {
  name: 'IndexPage',
  async asyncData({ $axios }) {
    const post = await $axios.$get(`http://nuxt.local/wp-json/nuxt-api/v1/front-page`)
    if (post.length === 0) throw({ statusCode: 404, message: 'Post not found' })
    return { post }
  },
  data: ()=>({
    clicked: false
  }),
  methods: {
    click(){
      this.clicked = true
      this.$router.push(`/${this.post.hero_post.slug}`)
    },
    imgHoverIn: () => gsap.to('.hero-post img', { height: '165%', ease: "Power4.easeOut", duration: 2 }),
    imgHoverOut: () =>  gsap.to('.hero-post img', { height: '135%', ease: "Power4.easeOut", duration: 2 })
  },
  mounted(){
    const hero = document.querySelector('.hero');
    const image = document.querySelector('.hero-post');
    gsap.timeline()
      .set(document.body, { backgroundColor: "#2A292D" })
      .set(hero.children, { y: '-100vh' })
      .set(image, { display: 'none' })
      .set(image.children[0], { height: '90%', top: '60%' })
      .set(hero, { display: 'grid' })
      .to(hero.children, { y: 0, stagger: 0.3, delay: 0.3, ease: "Power4.easeIn", duration: 1 })
      .fromTo(image, { display: 'initial', y: '-100vh' }, { y: 0, ease: "Power4.easeOut", duration: 2.5, delay: 0.1 })
      .to(image.children[0], { height: '135%', ease: "Power3.easeOut", duration: 2.5, top: "40%" }, '<')
  },
  transition: (to, from) => {
    let imgSectionPosition;
    const sections = document.querySelectorAll('.hero section:not(:nth-child(2))'); // sections excluding second
    const secondSection = document.querySelector('.hero section:nth-child(2)');
    const heroPost = document.querySelector('.hero-post');
    return {
      beforeLeave: (el, done) => {
        gsap.set(document.body, { backgroundColor: '#C6A7A4' });
        gsap.to(sections, { opacity: 0 });
        gsap.to(secondSection.firstElementChild, { opacity: 0, onComplete: done });
        imgSectionPosition = secondSection.lastElementChild.getBoundingClientRect();
      },
      leave: (el, done) => {
        const { width, height, left } = imgSectionPosition;
        gsap.timeline({ onComplete: done })
          .set(secondSection, { position: 'absolute', delay: 0.5 })
          .set(heroPost, { width, height, left })
          .to(heroPost, { width: '50vw', left: 0 })
          .to(heroPost.firstElementChild, { height: '170%', top: '35%', left: '52%' }, '<')
      },
      enter(){}
    }
  }
}
</script>
