<template>
  <div>
    <Header @add-post="addPost" />
    <div v-for="post in posts" :key="post.id">
      <AllPost :post="post" @del-post="deletePost" @edit-post="editPost" />
    </div>
  </div>
</template>

<script>
import AllPost from "../components/AllPost.vue";
import Header from "../components/Header.vue";
export default {
  components: { Header, AllPost },
  name: "IndexPage",
  data() {
    return {
      posts: [],
    };
  },
  mounted() {
    this.getAllPost();
  },
  methods: {
    async getAllPost() {
      try {
        const post = await this.$axios.$get("/posts");
        this.posts = post;
      } catch (error) {
        console.log(err);
      }
    },
    async addPost(item) {
      try {
        const newData = await this.$axios.$post("/posts", item);
        this.posts.unshift(newData);
      } catch (error) {
        console.log(err);
      }
    },
    async deletePost(post) {
      try {
        await this.$axios.$delete(`/posts/${post.id}`);
        this.posts = this.posts.filter((item) => item.id !== post.id);
      } catch (error) {
        console.error(err);
      }
    },

    async editPost(item) {
      if (!item.title) {
        this.deletePost(item);
      }
      try {
        await this.$axios.$put(`/posts/${item.id}`, item);
      } catch (err) {
        console.error(err);
      }
    },
  },
};
</script>
