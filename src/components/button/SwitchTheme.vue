<template>
  <!-- <input
    type="checkbox"
    class="toggle"
    checked
    data-toggle-theme="dark,light"
    data-act-class="ACTIVECLASS"
  /> -->
  <button
    class="btn btn-primary btn-circle"
    checked
    data-toggle-theme="dark,light"
    data-act-class="ACTIVECLASS"
  >
    <i class="bi bi-brightness-high"></i>
  </button>
</template>

<script setup>
import { onMounted } from "vue";
import { themeChange } from "theme-change";

onMounted(() => {
  themeChange(false);
});

function clk_changeTheme() {
  console.log("Theme Change");
  console.log(document.querySelector("[data-toggle-theme]"));
}
</script>

<style scoped></style>
