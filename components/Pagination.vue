<template>
  <div class="pagination-container">
    <div class="pagination">
      <div v-for="(page, index) in pageNumbers" :key="index">
        <button
          v-if="index > 0"
          class="pagination-button"
          @click="changePage(index - 1)"
        >
          {{ index }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Pagination",
  props: ['pageNumbers'],
  methods: {
    changePage(index) {
      this.$emit("changePage", index)
    }
  }
}
</script>

<style lang="scss" scoped>
.pagination {
  display: flex;
  max-height: 100px;

  &-container {
    display: grid;
    width: 100%;
  }

  &-button {
    padding: 10px;
    place-items: center;
    border-radius: 10px;
    position: relative;
    left: 530px;
    background: rgb(161, 160, 160);
    cursor: pointer;
    &:hover {
      background: crimson;
    }
  }
}
</style>
