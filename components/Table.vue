<template>
  <table>
    <thead>
    <tr>
      <th>Id</th>
      <th>UserName</th>
      <th>Title</th>
      <th>Completed</th>
    </tr>
    </thead>
    <tbody>
    <tr v-for="(item, index) in items" :key="index">
      <td>{{ item.id }}</td>
      <td>{{ adapterUserNameById(item.userId) }}</td>
      <td>{{ item.title }}</td>
      <td>{{ item.completed ? '✓' : '⤫' }}</td>
    </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  name: 'Table',
  props: ['items', 'users'],
  methods: {
    adapterUserNameById(id){
      if (!this.users.length) return;

      return this.users.find((user) => user.id === id).username
    }
  },
}
</script>
<style lang="scss" scoped>
table {
  width: 70%;
  margin: 0 auto;
  border-spacing: 0;

  td, th {
    border-bottom: 0.1rem solid rgba(0, 0, 0, 0.05);
  }

  th {
    padding: 2.0rem 1.0rem;
    text-align: center;
  }

  td {
    text-align: center;
    padding: 0.5rem 1.0rem;
    transition: all 0.3s ease;
  }

  tr:hover td {
    background: rgba(0, 0, 0, 0.03);
  }
}
</style>
