<template>
    <div class="navbar">
        <div class="navbar_btns">
            <my-button @click="$router.push('/')" >Главная</my-button>
           <my-button @click="$router.push('/posts')" >Посты</my-button>
           <my-button @click="$router.push('/about')">О Диме</my-button>
           <my-button @click="$router.push('/store')">Store</my-button>
            <my-button @click="$router.push('/composition')">API</my-button>

        </div>


    </div>
</template>

<script>

    export default {

    }
</script>

<style scoped>
    .navbar {
        height: 50px;
        background-color: teal;
        box-shadow: 2px 2px 4px gray;
        display: flex;
    }
</style>