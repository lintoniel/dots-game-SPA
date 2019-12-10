<template>
  <div class="game__field">
    <div v-for="i in 5" :key="i">
      <div class="inline_block game__field_item"
        v-for="i in 5"
        :key="i"
      ></div>
    </div>
  </div>
</template>

<script>
export default {
  
}
</script>

<style scoped>

</style>