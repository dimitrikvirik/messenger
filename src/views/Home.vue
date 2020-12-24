<template>
  <div>

  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return{
      islogged: false
    }
  },
  mounted() {
    if(!this.islogged){
      window.location.href = "/login";
    }
  }
}
</script>

<style scoped>

</style>