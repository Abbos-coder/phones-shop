<template>
  <v-row>
    <v-col
      v-for="(file, idx) in files"
      :key="idx"
      class="d-flex child-flex"
      cols="4"
    >
      <v-img
        :src="`http://localhost:5050/${file.productImg.path}`"
        :lazy-src="`http://localhost:5050/${file.productImg.path}`"
        aspect-ratio="1"
        class="grey lighten-2"
      >
        <template v-slot:placeholder>
          <v-row class="fill-height ma-0" align="center" justify="center">
            <v-progress-circular
              indeterminate
              color="grey lighten-5"
            ></v-progress-circular>
          </v-row>
        </template>
      </v-img>
    </v-col>
  </v-row>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    let files = await $axios.$get("http://localhost:5050/api/upload");
    return { files };
  },
  data: () => ({
    users: [{id: 1, name: 'Abbos', age: 24}]
  })
};
</script>

<style></style>
