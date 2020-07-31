<template>
  <v-app>
    <v-app-bar color="primary" app dark dense clipped-left>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>Reddit</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-filter</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-dots-vertical</v-icon>
      </v-btn>
    </v-app-bar>

    <v-navigation-drawer app v-model="drawer" absolute clipped mini-variant>
      <v-list nav dense>
        <v-list-item-group>
          <v-list-item>
            <v-list-item-title>Foo</v-list-item-title>
          </v-list-item>

          <v-list-item>
            <v-list-item-title>Bar</v-list-item-title>
          </v-list-item>

          <v-list-item>
            <v-list-item-title>Fizz</v-list-item-title>
          </v-list-item>

          <v-list-item>
            <v-list-item-title>Buzz</v-list-item-title>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <v-container>
        <template v-if="page == null">
          <v-skeleton-loader type="list-item" />
          <v-skeleton-loader type="list-item" />
          <v-skeleton-loader type="list-item" />
          <v-skeleton-loader type="list-item" />
          <v-skeleton-loader type="list-item" />
        </template>
        <Preview
          v-else
          v-for="item in page.data.children"
          :subreddit="item.data.subreddit"
          :title="item.data.title"
          :preview="item.data.preview"
          :key="item.data.name"
        />
      </v-container>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import Vue from "vue";
import { Component } from "vue-property-decorator";
import Preview from "@/components/Preview.vue";
import Axios from "axios";

@Component({ components: { Preview } })
export default class App extends Vue {
  drawer = false;

  page: { data: { children: { data: unknown; kind: "t3" }[] } } | null = null;

  async mounted() {
    this.page = (
      await Axios.get(
        "https://cors-anywhere.herokuapp.com/https://reddit.com/r/popular.json"
      )
    ).data;
  }
}
</script>
