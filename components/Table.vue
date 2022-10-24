<template>
  <div class="table-container">
    <table class="table">
      <thead>
      <tr>
        <th v-for="column in columns" :key="column.id">{{ column.title }}</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="item in data" :key="item[columns[0].id]">
        <td v-for="column in columns" :key="`${item[columns[0].id]}-${column.id}`">{{ item[column.id] }}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'Table',
  props: ['columns', 'data'],
}
</script>
<style lang="scss" scoped>
.table-container{
  max-width: 1200px;
  padding: 0 15px;
  margin: 10px auto;
}

.table {
  width: 100%;
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
