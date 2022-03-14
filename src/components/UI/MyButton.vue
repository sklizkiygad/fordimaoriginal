<template>
    <button>
        <slot></slot>

    </button>
</template>

<script>
    export default {
        name:'my-button'

    }
</script>

<style scoped>
    button{
        background-color: crimson;
        color: aliceblue;
        outline: none;
        border: none;
        cursor: pointer;
        padding: 10px;
        margin: 5px;
    }
</style>