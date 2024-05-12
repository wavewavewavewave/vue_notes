<template>
  <div class="message">
    <p>Error! {{ message }}</p>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Message",
  props: {
    message: {
      type: String,
      required: true,
      default: ''
    }
  }
}
</script>

<style scoped >
.message {
  text-align: center;
}

p {
  color: red;
}
</style>