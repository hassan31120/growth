<template>
  <main role="main" class="main-content">
    <div class="container-fluid">
      <h2 class="h5 page-title pb-5">إضافة قسم جديد</h2>

      <form @submit.prevent="saveForm">
        <div class="card shadow mb-4">
          <div class="card-header">
            <strong class="card-title">إضافة قسم جديد</strong>
          </div>
          <div class="card-body">
            <div class="row">
              <div class="col-md-6 align-self-center">
                <div class="form-group mb-3">
                  <label for="simpleinput">الإسم</label>
                  <input
                    type="text"
                    id="simpleinput"
                    class="form-control"
                    v-model="this.form.name"
                  />
                  <span class="text-danger" v-if="errors.name">{{ errors.name[0] }}</span>
                </div>
                <div class="form-group mb-3">
                  <label for="example-email">الصورة</label>
                  <input
                    type="file"
                    id="example-email"
                    name="example-email"
                    class="form-control"
                    ref="file"
                    @change="selectFile"
                  />
                  <span class="text-danger" v-if="errors.image">{{
                    errors.image[0]
                  }}</span>
                </div>
                <button
                  type="submit"
                  class="btn"
                  style="
                    background-color: #0c88c8;
                    color: aliceblue;
                    width: 100px;
                    font-weight: 600;
                  "
                >
                  تأكيد
                </button>
              </div>
              <!-- /.col -->
              <div class="col-md-6">
                <img src="@/assets/hi.gif" alt="" />
              </div>
            </div>
          </div>
        </div>
      </form>
    </div>
  </main>
</template>

<script>
export default {
  name: "add_cat",
  data() {
    return {
      form: {
        name: "",
        image: "",
      },
      errors: [],
    };
  },
  mounted() {},
  methods: {
    alert() {
      var toastMixin = this.$swal.mixin({
        toast: true,
        icon: "success",
        title: "General Title",
        animation: false,
        position: "top-right",
        showConfirmButton: false,
        timer: 3000,
        timerProgressBar: true,
        didOpen: (toast) => {
          toast.addEventListener("mouseenter", this.$swal.stopTimer);
          toast.addEventListener("mouseleave", this.$swal.resumeTimer);
        },
      });
      toastMixin.fire({
        animation: true,
        title: "تم إضافة القطاع بنجاح",
      });
    },
    async saveForm() {
        await axios
        .post(`api/cat/add`, this.form, {
          headers: {
            Accept: "application/json",
            "Content-Type": "multipart/form-data",
          },
        })
        .then(() => {
          (this.form.name = ""), (this.form.image = ""), (this.$refs.file.value = null);
          this.$router.push({ name: "cats" });
          this.alert();
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },

    selectFile() {
      this.form.image = this.$refs.file.files[0];
    },
  },
};
</script>

<style></style>
