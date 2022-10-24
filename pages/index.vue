<template>
  <div>
    <div v-if="error">
      <h2>{{ error }}</h2>
    </div>
    <div v-if="!error">
      <Input
        v-model.lazy="searchValue"
      />
      <Table
        :columns="columns"
        :data="currentPageList"
      />
      <Pagination
        :total-pages="totalPages"
        :current-page="currentPageNumber"
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
      searchValue: '',
      todosList: [],
      usersList: {},
      filteredTodoList: [],
      columns: [
        {
          id: 'id',
          title: 'Id'
        },
        {
          id: 'userName',
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
      perPage: 25,
      error: null,
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
        const users = await this.$axios.$get('/users')
        this.usersList = users.reduce((acc, item) => {
          acc[item.id] = item
          return acc
        }, {})
      } catch (error) {
        this.error = error.response.data.message
      }
    },

    async getTodos() {
      try {
        this.todosList = (await this.$axios.$get('/todos')).map(todo => {
          return {
            ...todo,
            userName: this.usersList[todo.userId].username,
            completed: todo.completed ? '✓' : '⤫'
          }
        })
        this.filteredTodoList = [...this.todosList];
      } catch (error) {
        this.error = error.response.data.message
      }
    },

    setPage(index) {
      this.currentPageNumber = index;
    },

    filterItems() {
      if (!this.searchValue) {
        return this.filteredTodoList = [...this.todosList];
      }
      this.filteredTodoList = this.todosList.filter((item) => {
        return item.title.includes(this.searchValue);
      });
      this.currentPageNumber = 0;
    }
  },
  computed: {
    currentPageList() {
      const start = this.currentPageNumber * this.perPage;
      const end = start + 25
      return this.filteredTodoList.slice(start, end)
    },

    totalPages() {
      return Math.ceil(this.filteredTodoList.length / this.perPage)
    }
  },

  watch: {
    searchValue() {
      this.filterItems()
    }
  }
}
</script>
