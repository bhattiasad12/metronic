<script setup>
import axios from "axios";
import { onMounted, ref } from "vue";
import TheSideBar from "@/components/layout/TheSideBar.vue";
import TheHeader from "@/components/layout/TheHeader.vue";
import { useRouter, useRoute } from "vue-router";
import { useAuthStore } from "../../stores/auth";
import Swal from "sweetalert2";

const props = defineProps(["id"]);

const route = useRoute();
const router = useRouter();
const id = route.params.id;
const auth = useAuthStore();

const name = ref("");
const slug = ref("");
const description = ref("");
const price = ref("");

onMounted(async () => {
  await axios
    .get("http://127.0.0.1:8000/api/product/" + id)
    .then((res) => {
      name.value = res.data.name;
      slug.value = res.data.slug;
      description.value = res.data.description;
      price.value = res.data.price;
    })
    .catch((error) => {
      console.log(error);
    });
});

function submitForm() {
  axios
    .patch(
      "http://127.0.0.1:8000/api/product/" + id,
      {
        name: name.value,
        slug: slug.value,
        description: description.value,
        price: price.value,
      },
      { headers: { Authorization: `Bearer ${auth.token}` } }
    )
    .then(function (response) {
      const status = JSON.parse(response.status);

      if (status == "200") {
        Swal.fire({
          text: `Product has been successfully updated!`,
          icon: "success",
          showCancelButton: false,
          confirmButtonText: "Ok",
          buttonsStyling: false,
          customClass: {
            confirmButton: "btn btn-primary",
          },
        });
        router.push("/productindex");
      }
    })
    .catch(function (error) {
      console.log(error);
    });
}
</script>

<template>
  <body
    class="dark-mode header-fixed header-tablet-and-mobile-fixed toolbar-enabled toolbar-fixed aside-enabled aside-fixed"
    style="
      --kt-toolbar-height: 55px;
      --kt-toolbar-height-tablet-and-mobile: 55px;
    "
  >
    <div class="d-flex flex-column flex-root">
      <TheSideBar />
      <TheHeader />
      <div class="page d-flex flex-row flex-column-fluid">
        <div class="app-container container-xxl">
          <div class="wrapper d-flex flex-column flex-row-fluid">
            <div class="card card-flush py-4" style="margin-bottom: 4rem">
              <!--begin::Card header-->
              <div class="card-header border-0 pt-6">
                <div class="card-title"><h2>Product Edit</h2></div>
                <div class="car-toolbar">
                  <div class="d-flex justify-content-end">
                    <a
                      @click="$router.go(-1)"
                      id="kt_ecommerce_add_product_submit"
                      class="btn btn-primary"
                    >
                      Go Back
                    </a>
                  </div>
                </div>
              </div>
              <!--end::Card header--><!--begin::Card body-->
              <div class="card-body pt-0">
                <!--begin::Input group-->
                <form class="form" @submit.prevent="submitForm">
                  <div class="mb-5 fv-row fv-plugins-icon-container">
                    <!--begin::Label--><label class="required form-label"
                      >Product Name</label
                    ><!--end::Label--><!--begin::Input--><input
                      type="text"
                      class="form-control mb-2"
                      placeholder="Product name"
                      v-model="name"
                    /><!--end::Input--><!--begin::Description-->
                    <div class="text-muted fs-7">
                      A product name is required and recommended to be unique.
                    </div>
                    <!--end::Description-->
                    <div
                      class="fv-plugins-message-container invalid-feedback"
                    ></div>
                  </div>

                  <div class="mb-5 fv-row fv-plugins-icon-container">
                    <!--begin::Label--><label class="required form-label"
                      >Slug</label
                    ><!--end::Label--><!--begin::Input--><input
                      type="text"
                      v-model="slug"
                      class="form-control mb-2"
                      placeholder="Product Slug"
                    /><!--end::Input--><!--begin::Description-->
                    <div class="text-muted fs-7">eg product-name.</div>
                    <!--end::Description-->
                    <div
                      class="fv-plugins-message-container invalid-feedback"
                    ></div>
                  </div>
                  <div class="mb-5 fv-row fv-plugins-icon-container">
                    <!--begin::Label--><label class="required form-label"
                      >Product Description</label
                    ><!--end::Label--><!--begin::Input-->
                    <textarea
                      v-model="description"
                      class="form-control mb-2"
                      cols="30"
                      rows="3"
                      placeholder="product description"
                    ></textarea>
                    <div class="text-muted fs-7">A product description.</div>
                    <div
                      class="fv-plugins-message-container invalid-feedback"
                    ></div>
                  </div>
                  <div class="mb-5 fv-row fv-plugins-icon-container">
                    <!--begin::Label--><label class="required form-label"
                      >Price</label
                    ><!--end::Label--><!--begin::Input--><input
                      type="text"
                      v-model="price"
                      class="form-control mb-2"
                      placeholder="Price"
                    /><!--end::Input--><!--begin::Description-->
                    <div
                      class="fv-plugins-message-container invalid-feedback"
                    ></div>
                  </div>
                  <div class="fv-row fv-plugins-icon-container">
                    <div class="d-flex justify-content-end">
                      <a
                        @click="$router.go(-1)"
                        id="kt_ecommerce_add_product_cancel"
                        class="btn btn-light me-5"
                        >Cancel</a
                      >
                      <button
                        type="submit"
                        id="kt_ecommerce_add_product_submit"
                        class="btn btn-primary"
                      >
                        Save Changes
                      </button>
                    </div>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </body>
</template>
