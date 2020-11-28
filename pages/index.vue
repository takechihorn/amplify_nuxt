<template>
  <div class="container">
    <div>
      <Logo />
      <h1 class="title">
        amplify-nuxt
      </h1>
      <div v-if="!$auth.isAuthenticated">
        <nuxt-link to="/login" class="button--gray">Login</nuxt-link>
        <nuxt-link to="/register" class="button--green">Register</nuxt-link>
      </div>
      <div v-else>
        <p>You're logged in as {{ $auth.email }}</p>
        <button @click="$store.dispatch('auth/logout')" class="button--gray">
          Logout
        </button>
        <nuxt-link to="/posts/create" class="button--green"
          >Create Post</nuxt-link
        >
      </div>
      <div v-for="post in posts" :key="post.id" class="py-6">
        <h4 class="text-lg font-semibold">
          <nuxt-link :to="`/posts/${post.id}`">{{ post.title }}</nuxt-link>
        </h4>
        <p>{{ post.summary }}</p>
        <small>By:{{ post.author.email }}</small>
        <div v-if="post.author.id === $auth.id" class="justify-center text-xs">
          <nuxt-link :to="`/posts/${post.id}/edit`" class="mr-3"
            >Edit</nuxt-link
          >
          <button @click="deletePost(post.id)" class="text-red-500">
            Delete
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ store }) {
    return {
      posts: await store.dispatch("api/listPosts")
    };
  },
  methods: {
    async deletePost(id) {
      await this.$store.dispatch("api/deletePost", id);
      this.posts = await this.$store.dispatch("api/listPosts");
    }
  }
};
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
@apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
