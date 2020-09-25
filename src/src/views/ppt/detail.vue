<template>
  <div class="app-container">

  </div>
</template>

<script>

export default {
  components: {  },
  data() {
    return {

    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      this.listLoading = true
    },
  }
}
</script>
