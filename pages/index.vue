<template>
  <div class="table">
    <div v-if="error">
      <h2>{{ errorMessage }}</h2>
    </div>
    <div v-if="!error">
      <Input
        v-model="value"
        @input="filterItems"/>
      <Table
        :items="pageList"
        :users="usersList"
      />
      <Pagination
        :page-numbers="chuckedList"
        @changePage="setPage"
      />
    </div>
  </div>
</template>
<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      value: '',
      todosList: [],
      usersList: [],
      pageList: [],
      chuckedList: [],
      filterTodoList: [],
      error: false,
      errorMessage: '',
    }
  },
  async mounted() {
    try {
      this.todosList = (await this.$axios.$get('/todos'))
      this.filterTodoList = [...this.todosList];
      this.chuckedArray(this.todosList)
    } catch (error) {
      this.errorMessage = error.response.data.message;
      this.error = true;
    }

    try {
      this.usersList = (await this.$axios.$get('/users'))
    } catch (error) {
      this.errorMessage = error.response.data.message
      this.error = true;
    }

  },
  methods: {
    setPage(index) {
      this.pageList = this.chuckedList[index]
    },

    chuckedArray(array) {
      for (let i = 0; i <= array.length; i += 25) {
        this.chuckedList.push(array.slice(i, i + 25))
      }
      this.pageList = this.chuckedList[0]
    },

    filterItems() {
      this.filterTodoList = this.todosList.filter((item) => {
        return item.title.includes(this.value);
      });

      this.chuckedList = [];

      this.chuckedArray(this.filterTodoList);
    }
  },
}
</script>
<style scoped>
.table {
  width: 70%;
  margin: 0 auto;
}
</style>
