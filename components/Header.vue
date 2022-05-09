<template>
  <header>
    <NuxtLink to="/"><Logo v-bind:class="classObject" /></NuxtLink>
    <div class="cont" :class="{ active }">
      <div class="hamburger" @click="active = !active">
        <div class="circle"><div></div></div>
        <span></span>
        <span></span>
        <span></span>
      </div>
      <div class="menu" v-if="active">
        <LinkAnimation v-for="link in menu" :key="link.ID" :link="link" />
      </div>
    </div>
  </header>
</template>

<script>
import Logo from "~/assets/images/logo.svg?inline";
import LinkAnimation from './LinkAnimation.vue';

export default {
  components: { Logo, LinkAnimation },
  data: () => ({
    active: false,
    menu: {}
  }),
  async fetch() {
    const menu = await this.$axios.$get(`http://nuxt.local/wp-json/nuxt-api/v1/menu`);
    this.menu = Object.entries(menu).map(item => {
      const { ID, title, slug } = item[1];
      return { ID, title, slug };
    });
  },
  computed: {
    classObject: function () {
      return {
        pink: this.$route.path !== "/"
      }
    }
  }
};
</script>
