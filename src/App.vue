<template>
  <div>  
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: ''
    }
  },
  mounted() {
    fetch(`${process.env.VUE_APP_API_URL}`)
      .then(res => res.text())
      .then(data => {
        this.message = data
      })
  }
}
</script>