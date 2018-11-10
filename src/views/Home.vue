<template>
  <div class="home">
      <TopNav/>
      <TableToolBar/>
      <TableEdit/>
  </div>
</template>

<script>
// @ is an alias to /src
import TopNav from '@/components/TopNav.vue'
import TableEdit from '@/components/TableEdit.vue'

export default {
  name: 'home',
  components: {
      TopNav,
      TableEdit,
  }
}
</script>

<style scoped>

</style>