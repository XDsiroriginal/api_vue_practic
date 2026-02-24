<script>

  import axios from 'axios'

  export default {
    name: 'ProductCatalog',

    async mounted() {
      try {
        const response = await axios.get(`${process.env.VUE_APP_API_URL}/products`)
        console.log(response.data)
      } catch (error) {
        console.log('ошибка отправки get запроса на сервер')
      }
    }
  }


</script>

<template>

  <h1>console test</h1>

</template>

<style scoped>

</style>