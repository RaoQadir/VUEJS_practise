<template>
  <!-- button used to trigger event -->
  <div>
    <button v-on:click="alertDisplay">Delete</button>
  </div>
</template>
<script>
export default {
  props: ["array_key"],
  name: "Delete",
  methods: {
    alertDisplay() {
      // $swal function calls SweetAlert into the application with the specified configuration.
      this.$swal({
        title: "Are you sure To delete this?",
        type: "warning",
        showCancelButton: true,
        confirmButtonText: "OK",
        showCloseButton: false,
        showLoaderOnConfirm: false,
      }).then((isConfirm) => {
        if (isConfirm.isConfirmed == true) {
          this.$emit("delete_post", this.array_key);
        }
      });
    },
  },
};
</script>