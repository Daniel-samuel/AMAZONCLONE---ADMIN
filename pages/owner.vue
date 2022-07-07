<template>
  <main class="container-fluid c-section">
    <div class="row">
      <div class="col-sm-3"></div>
      <div class="col-sm-6">
        <div class="a-spacing-top-medium"></div>
        <h2>Add a new Owner</h2>
        <form>
          <!-- name -->
          <div class="a-spacing-top-medium">
            <label>Name</label>
            <input
              type="text"
              class="a-input-text"
              style="width: 100%"
              v-model="name"
            />
          </div>

          <!-- about -->
          <div class="a-spacing-top-medium">
            <label>About</label>
            <input
              type="text"
              class="a-input-text"
              style="width: 100%"
              v-model="about"
            />
          </div>

          <!-- photo -->
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
                <span class="a-button-text" @click="onAddOwner">Add owner</span>
              </span>
            </span>
          </div>
        </form>
        <br />
        <ul class="list-group-item">
          <li v-for="owner in owners" :key="owner._id">
            <hr />
            {{ owner.name }}
          </li>
        </ul>
      </div>
      <div class="col-sm-3"></div>
    </div>
  </main>
</template>
<script>
export default {
  async asyncData({ $axios }) {
    try {
      let response = await $axios.$get("http://localhost:8080/api/owner");
      return {
        owners: response.owners,
      };
    } catch (err) {
      console.log(err);
    }
  },

  data() {
    return {
      name: "",
      about: "",
      selectFile: null,
      fileName: "",
    };
  },
  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0];
      this.fileName = event.target.files[0].name;
    },
    async onAddOwner() {
      let data = new FormData();
      data.append("name", this.name);
      data.append("about", this.about);
      data.append("photo", this.selectedFile, this.selectedFile.name);

      //   let data1 = {
      //     name: this.name,
      //   };
      //   let data2 = {
      //     about: this.about,
      //   };

      let response = await this.$axios.$post(
        "http://localhost:8080/api/owner",
        data
      );

      this.owners.push(this.name);
    },
  },
};
</script>
