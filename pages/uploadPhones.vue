<template>
  <section class="upload-phone">
    <h1 class="text-center">Upload new phone</h1>
    <div class="upload-phone__body">
      <!-- 1 Line -->
      <v-row>
        <v-col class="mt-8">
          <v-col cols="12">
            <v-file-input
              :rules="rules"
              accept="image/*"
              placeholder="Pick an avatar"
              prepend-icon="mdi-camera"
              label="Avatar"
              v-model="phone.image"
              @change="productImage"
            ></v-file-input>
          </v-col>
          <v-col cols="12">
            <v-text-field
              label="Title"
              counter="60"
              v-model="phone.title"
            ></v-text-field>
          </v-col>
          <v-col cols="12">
            <v-select
              v-model="selectCategory"
              :items="brand"
              attach
              label="Gategory"
            ></v-select>
          </v-col>

          <!-- 2 Line -->
          <v-col cols="12">
            <v-select
              v-model="phone.brand"
              :items="brand"
              attach
              label="Brand"
            ></v-select>
          </v-col>
          <v-col cols="12">
            <v-text-field
              label="Price"
              counter="4"
              v-model="phone.price"
            ></v-text-field>
          </v-col>

          <!-- 3 Line -->
          <v-col cols="12">
            <v-select
              v-model="phone.status"
              :items="status"
              attach
              label="Status"
            ></v-select>
          </v-col>
          <v-col cols="12">
            <v-select
              v-model="phone.tags"
              :items="tagItems"
              attach
              label="Tags"
              multiple
              counter="6"
            ></v-select>
          </v-col>
        </v-col>
        <v-col class="mt-8" v-if="phone.image">
          <v-card class="mx-auto my-12" max-width="354">
            <div class="card-img">
              <v-img class="card--image" :src="product_image"></v-img>
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

              <div class="my-4 text-subtitle-1">$ • {{ phone.price }}</div>
              <div class="text-subtitle-1">Brand: {{ phone.brand }}</div>
            </v-card-text>

            <v-divider class="mx-4"></v-divider>

            <v-card-title>Tags for search</v-card-title>

            <v-card-text>
              <v-chip-group
                v-model="selection"
                active-class="deep-purple accent-4 white--text"
                column
              >
                <v-chip v-for="(tag, id) in phone.tags" :key="id">
                  {{ tag }}
                </v-chip>
              </v-chip-group>
            </v-card-text>

            <v-card-actions>
              <v-btn color="deep-purple lighten-2" text> Buy now </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
        <v-col class="d-flex justify-center align-center" v-else>
          <h4 class="text-center">No product !</h4>
        </v-col>
      </v-row>
      <v-col class="text-center my-8">
        <v-btn
          :loading="loading3"
          :disabled="loading3"
          color="blue-grey"
          class="ma-2 white--text"
          @click="submitProduct"
        >
          Upload
          <v-icon right dark> mdi-cloud-upload </v-icon>
        </v-btn>
      </v-col>
    </div>
  </section>
</template>

<script>
export default {
  data: () => ({
    selection: "",
    loading3: false,
    loader: null,
    rating: 0,
    product_image: null,
    rules: [
      (value) =>
        !value ||
        value.size < 5000000 ||
        "Image size should be less than 5 MB!",
    ],
    tagItems: ["apple", "samsung", "mi", "xs", "11 pro"],
    brand: ["Apple", "Samsung", "Xiomi Redmi", "Vivo", "Poco", "Realme"],
    status: ["Active", "No Active"],
    selectCategory: "",

    phone: {
      title: "",
      categoryId: "",
      brand: "",
      price: null,
      tags: ["apple"],
      status: "",
      image: null,
    },
  }),
  methods: {
    productImage() {
      const file = this.phone.image;
      this.product_image = URL.createObjectURL(file);
    },
    async submitProduct() {
      if (this.phone.image) {
        let formData = new FormData();
        formData.append("title", this.phone.title);
        formData.append("categoryId", this.phone.categoryId);
        formData.append("brand", this.phone.brand);
        formData.append("price", this.phone.price);
        formData.append("tags[]", this.phone.tags);
        formData.append("status", this.phone.status);
        formData.append("image", this.phone.image);
        console.log(formData);
        await this.$axios
          .post("/api/phones", formData, {
            headers: {
              "Content-Type": "multipart/form-data",
            },
          })
          .then((response) => {
            console.log(response);
            this.$toasted.success("Image uploaded and saved in mongoDB!", {
              theme: "bubble",
              position: "top-right",
              duration: 5000,
              icon: "done",
            });
          })
          .catch((error) => {
            console.log({ error });
            this.$toasted.error("Error try again ");
            console.log(this.phone);
          });
      } else {
        console.log("there are no files.");
      }
    },
  },
  watch: {
    selectCategory(e) {
      e == "Apple"
        ? (this.phone.categoryId = "61ed9f58819db0f9db9d9680")
        : null;
      e == "Poco" ? (this.phone.categoryId = "61ed9ceb819db0f9db9d9676") : null;
      e == "Ralmi"
        ? (this.phone.categoryId = "61ed9cdd819db0f9db9d9674")
        : null;
      e == "Samsung"
        ? (this.phone.categoryId = "61eda2c65c89c8a507402101")
        : null;
      e == "Vivo" ? (this.phone.categoryId = "61ed9dc1819db0f9db9d9679") : null;
      e == "Xiomi Redmi"
        ? (this.phone.categoryId = "61eda2d05c89c8a507402103")
        : null;
    },
    loader() {
      const l = this.loader;
      this[l] = !this[l];
      setTimeout(() => (this[l] = false), 1300);
      this.loader = null;
    },
  },
};
</script>

<style lang="scss" scoped>
.card-img {
  width: 100%;
  height: 270px;
}
.card--image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}
</style>
