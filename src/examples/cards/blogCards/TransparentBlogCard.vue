<script setup>
defineProps({
  image: {
    type: String,
    required: true,
  },
  title: {
    type: String,
    required: true,
  },
  id: {
    type: String,
    required: true,
  },
  action: {
    type: Object,
    route: String,
    color: String,
    label: String,
    default: () => ({
      route: "/",
      color: "success",
      label: "Pilih Kategori",
    }),
  },
});
function coba(params) {
  console.log(params)
}
</script>
<template>
  <div class="card card-plain">
    <div class="card-header p-0 position-relative">
      <a href="#" class="d-block blur-shadow-image">
        <img
        style="width: 100%;height: 160px"
          :src="image"
          :alt="title"
          :id="id"
          class="img-fluid shadow border-radius-lg"
          loading="lazy"
        />
      </a>
    </div>
    <div class="card-body px-0">
      <h5>
        <a href="#"  class="text-dark font-weight-bold">{{
          title
        }}</a>
      </h5>
      <a
        href="#"
        class="text-sm icon-move-right"
        :class="`text-${action.color}`"
        >{{ action.label }}
        <i class="fa fa-arrow-right" aria-hidden="true"></i>

      </a>
    </div>
  </div>
</template>
