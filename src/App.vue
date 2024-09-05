<template>
  <div id="app">
    <VTable 
      :usersList="usersList" 
      :headers="headers" 
      :limit="5">
    </VTable>
  </div>
</template>

<script>
import VTable from './components/v-table.vue';
import { useUsers } from './assets/users';
import { ref } from 'vue';

export default {
  name: 'App',
  components: {
    VTable
  },
  setup() {
    const usersList = ref('')
    usersList.value = useUsers();
    const headers = ref([
      'Id',
      'Фио',
      'Отдел',
      'ЗП',
      'Должность',
      'День рождения',
    ]);

    return {
      usersList,
      headers
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  justify-content: center;
}
</style>
