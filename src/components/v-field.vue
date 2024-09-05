<template>
    <td>{{ value }}</td>
</template>

<script>
    export default {
        name:'vField',
        props: {
            value: String | Number
        }
    }
</script>

<style lang="scss" scoped>
    td{
        padding: 5px;
        background-color: aliceblue;
        border-radius: 5px;
        border: 1px solid rgb(213, 213, 213);

        &:hover{
        border: 1px dotted black
            
        }
    }
</style>