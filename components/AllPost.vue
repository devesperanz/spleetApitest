<template>
  <!-- <div>
    <v-sheet
      outlined
      :class="{editing : post === }"
      class="pa-4 mb-4 mx-auto d-flex justify-space-between align-center"
      max-width="1200"
    >
      <h2>{{ post.title }}</h2>
      <div>
        <v-btn icon @click="editPost(todo)">
          <v-icon>mdi-square-edit-outline</v-icon>
        </v-btn>
        <v-btn icon @click="$emit('del-post', post)">
          <v-icon>mdi-trash-can-outline</v-icon>
        </v-btn>
      </div>
    </v-sheet>
    <div class="edit">
      <v-text-field
        @keyup.enter="doneEdit(post)"
        @blur="doneEdit(post)"
        filled
        v-model="post.title"
        solo
        height="65"
        hide-details="true"
      />
    </div>
  </div> -->
  <div :class="{ editing: post === editedPost }">
    <v-sheet
      outlined
      max-width="1200"
      class="d-flex justify-space-between align-center pa-4 mb-4 mx-auto add"
    >
      <h2>
        {{ post.title }}
      </h2>

      <div>
        <v-btn @click="editTodo(post)" text icon color="warning">
          <v-icon> mdi-square-edit-outline </v-icon>
        </v-btn>
        <v-btn icon @click="$emit('del-post', post)">
          <v-icon>mdi-trash-can-outline</v-icon>
        </v-btn>
      </div>
    </v-sheet>
    <v-sheet class="edit mx-auto mb-4" max-width="1200">
      <v-text-field
        @keyup.enter="doneEdit(post)"
        @blur="doneEdit(post)"
        v-model="post.title"
        solo
        height="65"
        hide-details="true"
      />
    </v-sheet>
  </div>
</template>

<script>
export default {
  props: {
    post: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      editedPost: {},
    };
  },
  methods: {
    doneEdit(post) {
      if (!this.editedPost) {
        return;
      }
      this.editedPost = null;
      this.$emit("edit-post", post);
    },
    editTodo(post) {
      this.editedPost = post;
    },
  },
};
</script>

<style scoped>
.edit {
  display: none;
}
.add {
  display: block;
}
.editing .edit {
  display: block;
}
.editing .add {
  display: none !important;
}
</style>
