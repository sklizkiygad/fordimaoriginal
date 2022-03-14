<template>
    <input :value="modelValue" @input="updateInput" type="text">
</template>

<script>
    export default {
        name:'my-input',
        props:{
            modelValue:[String,Number]
        },
        methods:{
            updateInput(event){
                this.$emit('update:modelValue',event.target.value);

            }
        }
        
    }

</script>

<style scoped>
    input{
        margin: 5px;
        padding: 5px;
        border:1px solid darkslategray;
    }

</style>