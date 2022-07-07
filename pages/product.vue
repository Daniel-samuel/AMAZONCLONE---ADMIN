<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium">
              <h2 style="text-align: center">Add a new Product</h2>
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
                  />
                </div>

                <!-- Description input -->
                <div class="a-spacing-top-medium">
                  <label style="margin-bottom: 0px" for="">Description</label>
                  <textarea
                    name=""
                    id=""
                    placeholder="provide details such as a product description"
                    style="width: 100%"
                    v-model="description"
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
                      <span class="a-button-text" @click="onAddProduct"
                        >Add product</span
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
  async asyncData({ $axios }) {
    try {
      let categories = $axios.$get("http://localhost:8080/api/category");
      let owners = $axios.$get("http://localhost:8080/api/owner");

      const [categoryResponse, ownerResponse] = await Promise.all([
        categories,
        owners,
      ]);
      //   console.log(categoryResponse);

      return {
        categories: categoryResponse.categories,
        owners: ownerResponse.owners,
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
      price: 0,
      description: "",
      stockQuantity: 1,
      selectFile: null,
      fileName: "",
    };
  },
  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0];
      this.fileName = event.target.files[0].name;
    },
    async onAddProduct() {
      let data = new FormData();
      data.append("title", this.title);
      data.append("price", this.price);
      data.append("description", this.description);
      data.append("ownerID", this.ownerID);
      data.append("stockQuantity", this.stockQuantity);
      data.append("categoryID", this.categoryID);
      data.append("photo", this.selectedFile, this.selectedFile.name);

      let result = await this.$axios.$post(
        "http://localhost:8080/api/products",
        data
      );
      this.$router.push("/");
    },
  },
};
</script>
