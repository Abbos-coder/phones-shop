<template>
  <section>
    <v-row class="d-flex justify-center mt-10">
      <v-col cols="6">
        <v-file-input
          :rules="rules"
          accept="image/*"
          placeholder="Pick an avatar"
          prepend-icon="mdi-camera"
          show-size
          label="Avatar"
          v-model="productImg"
        ></v-file-input>
      </v-col>
      <v-col cols="2">
        <v-btn depressed color="primary" @click="submitFiles"> upload </v-btn>
      </v-col>
    </v-row>
  </section>
</template>

<script>
export default {
  data: () => ({
    productImg: null,
    rules: [
      (value) =>
        !value ||
        value.size < 3000000 ||
        "Avatar size should be less than 3 MB!",
    ],
  }),
  methods: {
    async submitFiles() {
      try {
        if (this.productImg) {
          let formData = new FormData();
          formData.append("image", this.productImg);

          await this.$axios
            .post("/api/upload", formData, {
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

              this.productImg = null;
            })
            .catch((error) => {
              console.log({ error });
              // this.$toasted.error("Error try again ");
            });
        } else {
          console.log("there are no files.");
        }
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style></style>
