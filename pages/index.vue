<template>
  <div>
    <Header @add-post="addPost" />
    <div v-for="post in formattedPost" :key="post.id">
      <AllPost :post="post" @del-post="deletePost" @edit-post="editPost" />
    </div>
  </div>
</template>

<script>
import AllPost from "../components/AllPost.vue";
import Header from "../components/Header.vue";
import { v4 as uuidv4 } from "uuid";
const STORAGE_KEY = "post-storage";
export default {
  components: { Header, AllPost },
  name: "IndexPage",
  data() {
    return {
      posts: [],
    };
  },
  computed: {
    formattedPost() {
      return this.posts.map((item) => {
        return {
          ...item,
          id: uuidv4(),
        };
      });
    },
  },
  mounted() {
    this.getAllPost();
  },
  methods: {
    async getAllPost() {
      try {
        const post = await this.$axios.$get("/posts");
        this.posts = post;
        this.posts = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
      } catch (error) {
        console.error(err);
      }
    },
    async addPost(item) {
      try {
        const newData = await this.$axios.$post("/posts", item);
        this.posts.unshift(newData);
        localStorage.setItem(STORAGE_KEY, JSON.stringify(this.posts));
      } catch (error) {
        console.error(err);
      }
    },
    async deletePost(post) {
      try {
        await this.$axios.$delete(`/posts/${post.id}`);
        this.posts = this.formattedPost.filter((item) => item.id !== post.id);
        localStorage.setItem(STORAGE_KEY, JSON.stringify(this.posts));
      } catch (error) {
        console.error(err);
      }
    },

    async editPost(item) {
      if (!item.title) {
        this.deletePost(item);
      }
      console.log(item);
      item.title = item.title.trim();
      // try {
      //   await this.$axios.$put(`/posts/${item.id}`, item);
      //   // localStorage.setItem(STORAGE_KEY, JSON.stringify(this.posts));
      // } catch (err) {
      //   console.error(err);
      // }
    },
  },
};
</script>
