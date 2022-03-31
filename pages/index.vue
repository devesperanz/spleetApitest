<template>
  <div>
    <Header @add-todo="addTodo" />
    <div v-for="post in formattedPost" :key="post.id">
      <AllPost :post="post" @del-post="deletePost" @edit-post="editPost" />
    </div>
  </div>
</template>

<script>
import AllPost from "../components/AllPost.vue";
import Header from "../components/Header.vue";
const STORAGE_KEY = "todo-storage-key";
import { v4 as uuidv4 } from "uuid";
const THEME_KEY = "todo-theme-key";
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
      } catch (error) {
        this.jokes = [{ title: "No Posts found" }];
      }
    },
    saveList() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.jokes));
    },
    async addTodo(item) {
      try {
        const newData = await this.$axios.$post("/posts", item);
        this.posts.unshift(newData);
      } catch (error) {}
    },
    async deletePost(item) {
      try {
        const res = await this.$axios.$delete(`/posts/${item.id}`);
        if ((res.status = "200")) {
          this.posts.splice(item.id, 1);
        } else {
          return this.posts;
        }
      } catch (error) {}
    },
    async editPost(item) {
      if (!item.title) {
        this.deletePost(item);
      }
      console.log(item);
      try {
        await this.$axios.$put(`/posts/${item.id}`, item);
      } catch (err) {
        console.error(err);
      }
    },
  },
};
</script>
