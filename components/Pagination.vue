<template>
  <div class="pagination-container">
    <div
      v-if="pageNumbers > 1"
      class="pagination">
      <button class="pagination-button" @click="decreasePageNumber">&#8592</button>
      <div v-for="number in pageNumbers" :key="number">
        <button
          :class="{chosenButton: number === chosenPage + 1}"
          class="pagination-button"
          @click="changePage(number - 1)"
        >
          {{ number }}
        </button>
      </div>
      <button
        class="pagination-button" @click="increasePageNumber">&#8594
      </button>
    </div>
  </div>
</template>
<script>
export default {
  name: "Pagination",
  props: {
    pageNumbers: {
      default: 0,
      type: Number
    },
    chosenPage: {
      default: 0,
      type: Number
    }
  },
  methods: {
    changePage(index) {
      this.$emit("changePage", index)
    },
    increasePageNumber() {
      this.$emit("increasePageNumber")
    },
    decreasePageNumber() {
      this.$emit("decreasePageNumber")
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
    padding: 10px;
    margin: 0 5px;
    place-items: center;
    border-radius: 10px;
    background: rgb(161, 160, 160);
    cursor: pointer;

    &:hover {
      background: rgb(163, 161, 161);
    }
  }

  .chosenButton {
    border: solid 2px green;
  }
}
</style>
