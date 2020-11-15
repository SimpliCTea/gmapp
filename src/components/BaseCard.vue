<template>
  <div :class="'card ' + size">
    <p class="card-title">{{ title }}</p>
    <div class="card-text">
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: String,
    size: {
      type: String,
      default: "col-4"
    }
  }
};
</script>

<style></style>
