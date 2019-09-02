<template>
  <div>
    <div class="bb mb4">
      <div class="mb1">
        <span class="i">{{comment.id}}</span>
        <nuxt-link :to="'/user/' + comment.by">{{comment.by}}</nuxt-link>
        {{comment.time}} ago
      </div>
      <div class="f6" v-html="comment.text"></div>
      <div class="i f6 gray">kids: {{comment.kids}}</div>
    </div>
    <ul class="ml3">
      <comment v-for="id in comment.kids" :key="id" :id="id"></comment>
    </ul>
  </div>
</template>

<script>
import axios from "~/plugins/axios";
export default {
  name: "comment",
  data() {
    return {
      comment: {},
      kids: []
    };
  },
  props: ["id"],
  async beforeMount() {
    const response = await axios.get(`item/${this.id}.json`);
    this.comment = response.data;
  },
  async mounted() {
    if (this.comment.kids) {
      const idToPromise = id => axios.get(`item/${id}.json`);
      const kidPromises = this.comment.kids.map(idToPromise);
      const kidResponses = await Promise.all(kidPromises);
      this.kids = kidResponses.map(res => res.data);
    }
  }
};
</script>