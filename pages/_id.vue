<template>
  <section>
    <v-row>
      <v-col>
        <v-img
          :src="`http://localhost:5050/${phone.image.path}`"
          :lazy-src="`http://localhost:5050/${phone.image.path}`"
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
      <v-col>
        <h2 class="text-center my-8">{{ phone.title }}</h2>
        <p>category {{ phone.category.name }}</p>
        <p>price: $ {{ phone.price }}</p>
        <p>status is - {{ phone.status }}</p>
        <p>image size: {{ (phone.image.size / 1024).toFixed(2) + " kb" }}</p>
      </v-col>
    </v-row>
  </section>
</template>

<script>
export default {
  async asyncData({ $axios, params }) {
    let phone = await $axios.$get(
      `http://localhost:5050/api/phones/${params.id}`
    );
    return { phone };
  },
  data: () => ({}),
};
</script>

<style scoped>
.v-image {
  border-radius: 20px;
}
</style>
