<template>
  <v-card class="mb-5">
    <v-card-title v-text="title" style="word-break: normal;" />
    <v-card-subtitle v-text="subreddit" />
    <v-card-text>
      <v-img v-if="preview && preview.images[0]" :src="imageUrl" />
    </v-card-text>
  </v-card>
</template>

<script lang="ts">
import Vue from "vue";
import { Component, Prop } from "vue-property-decorator";

@Component
export default class Preview extends Vue {
  @Prop() subreddit!: string;
  @Prop() title!: string;
  @Prop() preview!: {
    images: { source: { url: string; width: number; height: number } }[];
  };

  get imageUrl() {
    console.log(this.preview.images[0].source.url.replace("&amp;", "&"));
    return this.preview.images[0].source.url.replace("&amp;", "&");
  }
}
</script>
