<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium">
              <h2 style="text-align: center">Update{{ product.title }}</h2>
              <form action="">
                <!-- category dropdown -->
                <div class="a-spacing-top-medium">
                  <label>Category</label>
                  <select class="a-select-option" v-model="categoryID">
                    <option
                      v-for="category in categories"
                      :value="category._id"
                      :key="category._id"
                    >
                      {{ category.type }}
                    </option>
                  </select>
                </div>
                <!-- owner dropdown -->
                <div class="a-spacing-top-medium">
                  <label>Owner</label>
                  <select class="a-select-option" v-model="ownerID">
                    <option
                      v-for="owner in owners"
                      :value="owner._id"
                      :key="owner._id"
                    >
                      {{ owner.name }}
                    </option>
                  </select>
                </div>
                <!-- title input -->
                <div class="a-spacing-top-medium">
                  <label style="margin-bottom: 0px" for="">Title</label>
                  <input
                    type="text"
                    class="a-input-text"
                    style="width: 100%"
                    v-model="title"
                    :placeholder="product.title"
                  />
                </div>

                <!-- price input -->
                <div class="a-spacing-top-medium">
                  <label style="margin-bottom: 0px" for="">Price</label>
                  <input
                    type="number"
                    class="a-input-text"
                    style="width: 100%"
                    v-model="price"
                    :placeholder="product.price"
                  />
                </div>

                <!-- stockQuantity input -->
                <div class="a-spacing-top-medium">
                  <label style="margin-bottom: 0px" for="">StockQuantity</label>
                  <input
                    type="number"
                    class="a-input-text"
                    style="width: 100%"
                    v-model="stockQuantity"
                    :placeholder="product.stockQuantity"
                  />
                </div>

                <!-- Description input -->
                <div class="a-spacing-top-medium">
                  <label style="margin-bottom: 0px" for="">Description</label>
                  <textarea
                    name=""
                    id=""
                    style="width: 100%"
                    v-model="description"
                    :placeholder="product.description"
                  ></textarea>
                </div>

                <!-- Photo input -->
                <div class="a-spacing-top-medium">
                  <label style="margin-bottom: 0px" for="">Photo</label>
                  <div class="a-row a-spacing-top-medium">
                    <label class="choosefile-button">
                      <i class="fal fa-plus"></i>
                      <input type="file" @change="onFileSelected" multiple />
                      <p style="margin-top: -70px">{{ fileName }}</p>
                    </label>
                  </div>
                </div>

                <!-- button -->
                <hr />
                <div class="a-spacing-top-large">
                  <span class="a-button-register">
                    <span class="a-button-inner">
                      <span class="a-button-text" @click="onUpdateProduct"
                        >Update product</span
                      >
                    </span>
                  </span>
                </div>
              </form>
            </div>
          </div>
        </div>
        <div class="col-sm-3"></div>
      </div>
    </div>
  </main>
</template>
<script>
export default {
  async asyncData({ $axios, params }) {
    try {
      let categories = $axios.$get(
        "https://amazon-double.herokuapp.com/api/category"
      );
      let owners = $axios.$get("https://amazon-double.herokuapp.com/api/owner");
      let product = $axios.$get(
        `https://amazon-double.herokuapp.com/api/products/${params.id}`
      );

      const [categoryResponse, ownerResponse, productResponse] =
        await Promise.all([categories, owners, product]);
      return {
        categories: categoryResponse.categories,
        owners: ownerResponse.owners,
        product: productResponse.product,
      };
    } catch (err) {
      console.log(err);
    }
  },

  data() {
    return {
      categoryID: null,
      ownerID: null,
      title: "",
      price: "",
      description: "",
      stockQuantity: "",
      selectFile: null,
      fileName: "",
    };
  },
  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0];
      this.fileName = event.target.files[0].name;
    },
    async onUpdateProduct() {
      let data = new FormData();
      data.append("title", this.title);
      data.append("price", this.price);
      data.append("description", this.description);
      data.append("ownerID", this.ownerID);
      data.append("stockQuantity", this.stockQuantity);
      data.append("categoryID", this.categoryID);
      data.append("photo", this.selectedFile, this.selectedFile.name);

      let result = await this.$axios.$put(
        `https://amazon-double.herokuapp.com/api/products/${this.$route.params.id}`,
        data
      );
      this.$router.push("/");
    },
  },
};
</script>
