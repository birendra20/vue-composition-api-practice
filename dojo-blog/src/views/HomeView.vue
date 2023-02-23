<template>
  <div>
    <h1>Home</h1>

    <div v-if="error">{{ error }}</div>
    <div v-if="posts.length">
      <PostList :posts="posts" />
    </div>
    <div v-else>Loading...</div>
  </div>
</template>

<script>
import { ref, computed, watch } from "vue";
import PostList from "../components/PostList.vue";

export default {
  name: "HomeView",
  components: { PostList },
  setup() {
    const posts = ref([]);
    const error = ref(null);

    const load = async () => {
      try {
        let data = await fetch("http://localhost:3000/posts");
        console.log("data", data);

        if (!data.ok) {
          throw Error("no data available");
        }

        posts.value = await data.json();
      } catch (err) {
        error.value = err.message;
      }
    };
    load();

    return { posts, error };
  },
};
</script>
