<template>
    <div class="search_container">
        <input @input="searchEmit" type="text" v-model="value" placeholder="Поиск...">
        <button @click="searchEmit">
            <iSearch></iSearch>
        </button>
    </div>
</template>

<script>
import iSearch from '@/assets/svg/i-search.vue';
import { ref } from 'vue';
    export default {
        name:'vSerach',
        props:{
            value: String
        },
        components: {
            'iSearch': iSearch
        },

        setup(_,{emit}) {
            const value = ref('')
            const searchEmit = () => {
                emit('onSearch',value.value)
            }
            return{
                value,
                searchEmit
            }
        }
    }
</script>

<style lang="scss" scoped>
    .search_container{
        display: flex;
        padding: 5px;
        border-radius: 5px;
        background-color: aliceblue;


        * {
            border: none;
            background-color: rgba(0, 0, 0, 0);

            :hover{
                border: none;
            }

        }
        input:focus{
            outline-width: 0;
        }
    }
</style>