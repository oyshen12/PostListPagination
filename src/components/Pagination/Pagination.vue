<template>
  <div>
    <v-list>
      <Post
        v-for="post in pagePosts"
        :key="post.id"
        :post="post"
        @click.native="activePost = post.id"
        :active="activePost === post.id"
      />
    </v-list>
    <v-pagination
      v-if="paginationLength"
      v-model="page"
      :length="paginationLength"
      class="mt-4 mb-8"
    ></v-pagination>
    <div v-else class="post__empty">Ничего не найлено :(</div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import Post from "../Post/Post.vue";
import { post } from "@/store/storeTypes";

export default Vue.extend({
  name: "App",
  components: { Post },
  props: {
    posts: {
      type: Array as () => post[],
      required: true,
    },
  },
  data: () => ({
    page: 1,
    postOnPage: 10,
    activePost: "",
  }),
  computed: {
    pagePosts(): post[] {
      if (this.posts.length <= this.postOnPage) {
        return this.posts;
      }
      const index = this.page * this.postOnPage;
      return this.posts.slice(index - this.postOnPage, index);
    },
    paginationLength(): number {
      return Math.ceil(this.posts.length / this.postOnPage);
    },
    pageComp(): number {
      return this.paginationLength;
    },
  },
});
</script>

<style scoped lang="scss">
.post__empty {
  font-size: 24px;
  line-height: 28px;
  text-align: center;
}
</style>

