<template>
  <div class="landing-page">
    <login-page v-if="view == 'login'" @changeView="changeView"></login-page>
    <blog-page v-if="view == 'blog'"></blog-page>
  </div>
</template>

<script>
import LoginPage from '@/components/LoginPage.vue';
import BlogPage from '@/components/BlogPage.vue';
export default {
  name: 'LandingPage',
  components : { LoginPage, BlogPage },
  data() {
    return {
        view: 'login'
    };
  },
  methods: {
    changeView(){
      this.view = "blog";
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
