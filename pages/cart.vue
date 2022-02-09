<template>
  <section>
    <h1 class="text-center my-3">Your selected products !</h1>
    <h4 v-if="!phones" class="text-center mt-16 mb-6">No product yet !</h4>
    <v-row v-else>
      <v-col v-for="(phone, idx) in phones" :key="idx" cols="12" md="4">
        <v-card class="mx-auto mt-12 mb-4" max-width="324">
          <div class="card-img">
            <v-img
              class="card--image"
              :src="`http://localhost:5050/${phone.image.path}`"
              :lazy-src="`http://localhost:5050/${phone.image.path}`"
            ></v-img>
          </div>

          <v-card-title>{{ phone.title }}</v-card-title>

          <v-card-text>
            <v-row align="center" class="mx-0">
              <v-rating
                v-model="rating"
                color="amber"
                dense
                half-increments
                size="14"
              ></v-rating>

              <div class="grey--text ms-4">{{ rating }} (0)</div>
            </v-row>

            <div class="mt-3 text-subtitle-1">$ • {{ phone.price }}</div>
            <div class="text-subtitle-1">Brand: {{ phone.brand }}</div>
          </v-card-text>
          <v-card-actions>
            <v-btn color="deep-purple lighten-2"> Buy now </v-btn>
            <v-spacer></v-spacer>
            <v-btn color="deep-purple lighten-2" text @click="deletePhone(idx)">
              remove
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </section>
</template>

<script>
export default {
  data: () => ({
    phones: [],
    selection: "",
    rating: 4.5,
  }),
  mounted() {
    this.phones = this.$store.state.cart;
  },
  methods: {
    deletePhone(phone) {
      this.phones.splice(phone, 1);
    },
  },
};
</script>

<style scoped>
.card-img {
  height: 274px !important;
  overflow: hidden;
}
.card-img img {
  height: 100%;
  object-fit: cover;
  object-position: center;
}
</style>
