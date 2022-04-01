<template>
  <div class="mx-10 mt-5 mb-10">
    <h3 class="text-h4 font-weight-bold success--text mb-6">Dummy Posts</h3>
    <v-sheet max-width="884px" class="mx-auto">
      <v-text-field
        solo
        height="65"
        placeholder="Add New Posts ..."
        hide-details="true"
        v-model="title"
        @keyup.enter="addTodo"
      />
    </v-sheet>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: "",
    };
  },
  methods: {
    addTodo(e) {
      e.preventDefault();
      if (this.title === "") {
        return;
      }
      const newPost = {
        title: this.title,
      };
      this.$emit("add-post", newPost);
      this.title = "";
    },
  },
};
</script>
<style lang="scss" scoped></style>
