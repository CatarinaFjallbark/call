<template>
  <h1 class="header">{{text}}</h1>
</template>


<script>
  export default {
    name: 'MyHeader',
    props: {
      text: {
        type: String,
        required: true,
      },
    },
  };
</script>


<style scoped>
  .header {
    color: rgb(37, 37, 73);
  }

</style>