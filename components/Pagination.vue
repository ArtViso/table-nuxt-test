<template>
  <div class="pagination-container">
    <div class="pagination">
      <button
        class="pagination-button"
        :disabled="currentPage === 0"
        @click="decreasePageNumber">&#8592
      </button>
      <div v-for="number in totalPages" :key="number">
        <button
          :class="{active: number === currentPage + 1}"
          class="pagination-button"
          @click="changePage(number - 1)"
        >
          {{ number }}
        </button>
      </div>
      <button
        class="pagination-button"
        :disabled="currentPage === totalPages - 1"
        @click="increasePageNumber">&#8594
      </button>
    </div>
  </div>
</template>
<script>
export default {
  name: "Pagination",
  props: {
    totalPages: {
      default: 0,
      type: Number
    },
    currentPage: {
      default: 0,
      type: Number
    }
  },
  methods: {
    changePage(index) {
      this.$emit("changePage", index)
    },

    increasePageNumber() {
      if (this.currentPage === this.totalPages - 1) {
        return
      }
      this.$emit("changePage", this.currentPage + 1)
    },

    decreasePageNumber() {
      if (this.currentPage === 0) {
        return
      }
      this.$emit("changePage", this.currentPage - 1)
    }
  }
}
</script>
<style lang="scss" scoped>
.pagination {
  display: flex;
  max-height: 100px;
  justify-content: center;
  align-items: center;
  margin: 30px 0;

  &-container {
    width: 100%;
  }

  &-button {
    width: 40px;
    height: 40px;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0 5px;
    place-items: center;
    background: white;
    border: solid 1px black;
    cursor: pointer;
    transition: 0.5s;

    &:hover {
      border: solid 1px #6eb3ff;
      color: #6eb3ff;
    }

    &:disabled {
      border: solid 1px #d9d9d9;
      color: #d9d9d9;
      cursor: not-allowed;
      background: white;
      transition: 0.5s;
    }
  }

  .active {
    border: solid 1px #1890ff;
    color: #1890ff;
  }
}
</style>
