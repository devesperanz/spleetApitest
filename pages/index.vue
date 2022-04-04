<template>
  <div>
    <Header @add-post="addPost" />
    <div v-for="post in posts" :key="post.id">
      <AllPost :post="post" @del-post="deletePost" @edit-post="editPost" />
    </div>
  </div>
</template>

<script>
import vuetifyToast from "vuetify-toast";
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
        vuetifyToast.success("Post successfully fetched");
      } catch (error) {
        vuetifyToast.error("Something went wrong");
      }
    },
    async addPost(item) {
      try {
        const newData = await this.$axios.$post("/posts", item);
        this.posts.unshift(newData);
        vuetifyToast.success("You have successfully added a post.");
      } catch (error) {
        vuetifyToast.error("Something went wrong");
      }
    },
    async deletePost(post) {
      try {
        await this.$axios.$delete(`/posts/${post.id}`);
        this.posts = this.posts.filter((item) => item.id !== post.id);
        vuetifyToast.success("You have successfully deleted a post.");
      } catch (error) {
        vuetifyToast.error("Something went wrong");
      }
    },

    async editPost(item) {
      if (!item.title) {
        this.deletePost(item);
      }
      try {
        await this.$axios.$put(`/posts/${item.id}`, item);
        vuetifyToast.success("You have successfully edited a post.");
      } catch {
        vuetifyToast.error("Something went wrong");
      }
    },
  },
};
</script>
