<template>
  <div class="table">
    <div v-if="error">
      <h2>{{ errorMessage }}</h2>
    </div>
    <div v-if="!error">
      <Input
        v-model.lazy="value"
      />
      <Table
        :columns="columns"
        :data="pageList"
      />
      <Pagination
        :page-numbers="chuckedList.length"
        :chosen-page="currentPageNumber"
        @changePage="setPage"
        @increasePageNumber="increasePageNumber"
        @decreasePageNumber="decreasePageNumber"
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
      columns: [
        {
          id: 'id',
          title: 'Id'
        },
        {
          id: 'username',
          title: 'UserName'
        },
        {
          id: 'title',
          title: 'Title'
        },
        {
          id: 'completed',
          title: 'Completed'
        }
      ],
      error: false,
      errorMessage: '',
      currentPageNumber: 0,
    }
  },

  async fetch() {
    await this.getUsers();
    await this.getTodos();
  },

  methods: {
    async getUsers() {
      try {
        this.usersList = (await this.$axios.$get('/users'))
        console.log(this.usersList)
      } catch (error) {
        this.errorMessage = error.response.data.message
        this.error = true;
      }
    },

    async getTodos() {
      try {
        this.todosList = (await this.$axios.$get('/todos')).map(todo => {
          console.log(this.usersList)
          return {
            ...todo,
            userName: this.usersList.find((user) => user.id === todo.userId).username,
            completed: todo.completed ? '✓' : '⤫'
          }
        })
        this.filterTodoList = [...this.todosList];
        this.chuckedArray(this.todosList)
      } catch (error) {
        this.errorMessage = error.response.data.message;
        this.error = true;
      }
    },

    setPage(index) {
      this.currentPageNumber = index;
      this.pageList = this.chuckedList[index]
    },
    increasePageNumber() {
      if (this.currentPageNumber === this.chuckedList.length - 1) return
      this.currentPageNumber += 1;
      this.pageList = this.chuckedList[this.currentPageNumber];
    },
    decreasePageNumber() {
      if (this.currentPageNumber === 0) return
      this.currentPageNumber -= 1;
      this.pageList = this.chuckedList[this.currentPageNumber];
    },
    chuckedArray(array) {
      for (let i = 0; i <= array.length; i += 25) {
        this.chuckedList.push(array.slice(i, i + 25))
      }
      this.chuckedList = this.chuckedList.filter(arr => arr.length)
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
  watch: {
    value() {
      this.filterItems()
    }
  }
}
</script>
<style scoped>
.table {
  width: 70%;
  margin: 0 auto;
}
</style>
