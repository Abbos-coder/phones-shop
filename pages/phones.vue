<template>
  <section>
    <v-row>
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
            <v-btn color="deep-purple lighten-2" text @click="toCart(phone)">
              to Cart
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </section>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    let phones = await $axios.$get("http://localhost:5050/api/phones");
    return { phones };
  },
  data: () => ({
    selection: "",
    rating: 4.5,
  }),
  methods: {
    async toCart(phone) {
      const store = this.$store.state.cart;

      let formData = new FormData();
      formData.append("title", phone.title);
      formData.append("categoryId", phone.category._id);
      formData.append("brand", phone.brand);
      formData.append("price", phone.price);
      formData.append("tags[]", phone.tags);
      formData.append("status", phone.status);
      formData.append("image", phone.image);
      console.log(formData);

      await this.$axios
        .$post("/api/cart", formData, {
          headers: {
            "Content-Type": "multipart/form-data",
          },
        })
        .then((res) => {
          store.push(phone);
          console.log(res, "success !");
          this.$toasted.success("Your product added to your cart", {
            theme: "bubble",
            position: "top-right",
            duration: 5000,
            icon: "done",
          });
        })
        .catch((error) => {
          console.log(error);
          console.log(formData);
          this.$toasted.error(error);
        });
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
