<script>
import { computed, onMounted, reactive, ref } from 'vue';
import vField from './v-field.vue';
import vSearch from './v-search.vue';
import { useUsers } from '@/assets/users';
export default {
  name: 'vtable',
  components: {
    'vField': vField,
    'vSearch': vSearch
  },
  props: {
    headers: Array,
    limit: Number,
  },
  setup(props) {
    const page = ref(1)
    const searchValue = ref('')
    const usersList = ref([])
    usersList.value = useUsers();

    const userInput1 = ref('');
    const userInput2 = ref('');
    const userInput3 = ref('');
    const userInput4 = ref('');
    const userInput5 = ref('');

    const formData = reactive({
      id: '',
      fio: '',
      department: '',
      salary: '',
      job: '',
      Born: '',
    })

    formData.id = ''

    const userRemoveValue = ref(0)
    
    const removeUserForm = () => {
      console.log(usersList.value)
      usersList.value.splice(userRemoveValue.value-1,1)
    }

    const currUsersList = computed(() => {
      if (searchValue !== '') {
        return usersList.value
          .filter((item) => item.fio.includes(searchValue.value))
          .slice(((page.value - 1) * props.limit), (page.value) * props.limit)
      }
      return usersList.value.slice(((page.value - 1) * props.limit), (page.value) * props.limit)
    })

    const userSearch = (payload) => {
      searchValue.value = payload
    }
    const addUserForm = () => {
      usersList.value.push({
        id: usersList.value.length + 1,
        fio: userInput1,
        department: userInput2,
        salary: userInput3,
        job: userInput4,
        Born: userInput5,
      })
    }

    return {
      page,
      usersList,
      currUsersList,
      userInput1,
      userInput2,
      userInput3,
      userInput4,
      userInput5,
      userRemoveValue,
      userSearch,
      addUserForm,
      removeUserForm
    }
  }

}
</script>

<template>
  <div class="content">
    <div class="table_container">
      <div class="table_header_container">
        <div style="color:white">Сотрудники {{ page }} {{ usersList.length }}</div>
        <vSearch @onSearch="userSearch"></vSearch>
      </div>
      <table>
        <tr>
          <th style="color:white" v-for="header in headers">{{ header }}</th>
        </tr>
        <tr v-for="row in currUsersList" :key="row.id">
          <vField v-for="field in row" :key="field" :value="field"></vField>
        </tr>
      </table>
      <div class="pagination_container">
        <button v-if="page > 1" @click="() => { page-- }">/\</button>
        <button v-if="currUsersList.length === 5" @click="() => { page++ }">\/</button>
      </div>
    </div>
    <div class="form_container">
      <div>
        <h3>Add form:</h3>
        <div class="input_container">
          <div>Фио:<input type="text" v-model="userInput1" placeholder="Фио"></input></div>
          <div>Отдел:<input type="text" v-model="userInput2" placeholder="Отдел"></input></div>
          <div>Зп:<input type="text" v-model="userInput3" placeholder="Зп"></input></div>
          <div>Должность:<input type="text" v-model="userInput4" placeholder="Должность"></input></div>
          <div>День рождения:<input type="date" v-model="userInput5" placeholder="День рождения"></input></div>
          <div>
            <button @click="addUserForm">Add</button>
          </div>
        </div>
      </div>
      <div>
        <h3>Remove form</h3>
        <div class="input_container">
          <div >
          <select v-model="userRemoveValue">
            <option value="">Пользователь</option>
            <option v-for="row in usersList" :value="row.id">{{ row.id }}</option>
          </select>
        </div>
        <div>
          <button @click="removeUserForm">Remove</button>
        </div>
        </div>
      </div>
    </div>

  </div>
</template>

<style lang="scss" scoped>
.content {
  display: flex;
  width: 20rm;
  flex-direction: column;
  align-items: center;
  row-gap: 20px;

  .table_container {
    display: flex;
    flex-direction: column;
    padding: 20px;
    width: fit-content;
    border-radius: 10px;
    background-color: rgb(99, 99, 99);
    align-items: center;
    row-gap: 20px;
    box-shadow: 15px 10px 10px rgba(0, 0, 0, 0.093);

    .table_header_container {
      display: flex;
      width: 100%;
      justify-content: space-between;
    }

    .pagination_container {
      display: flex;
      flex-direction: row;
      justify-content: center;
      align-items: center;
      column-gap: 5px;
      background-color: aliceblue;
      border-radius: 5px;
    }

    button {
      width: 30px;
      height: auto;
      border: none;
      user-select: none;
      border-radius: 10px;
      background-color: rgba(0, 0, 0, 0);
    }
  }

  .form_container {
    display: flex;
    flex-direction: row;
    padding: 20px;
    border-radius: 10px;
    background-color: rgb(99, 99, 99);
    align-items: center;
    row-gap: 20px;
    box-shadow: 15px 10px 10px rgba(0, 0, 0, 0.093);
    color: white;

    div {
      width: 80%;
    }

    .input_container {
      display: flex;
      flex-flow: row wrap;
      color: black;

      div {
        margin: 5px;
        background-color: rgb(255, 255, 255);
        padding: 5px;
        border-radius: 5px;
        width: fit-content;

        input:focus {
          outline-width: 0;
        }

        * {
          border: none;
          background-color: rgba(0, 0, 0, 0);
        }
      }
    }
  }


}
</style>