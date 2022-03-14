<template>
    <div>
        <h1>Создание поста</h1>
        <form @submit.prevent>
            <my-input
                    v-focus
                    v-model="post.title"
                    type="text"
                    placeholder="Название"/>
            <my-input
                    v-model="post.body"
                    type="text"
                    placeholder="Описание"/>
            <my-button
             @click="createPost"
            >Закинуть
            </my-button>
        </form>
    </div>
</template>

<script>
    import MyButton from "./UI/MyButton";
    import MyInput from "./UI/MyInput";
    export default {
        components:{
            MyInput,
            MyButton
        },
        data(){
            return{
                post:{
                    title:'',
                    body:''


                }
            }
        },
        methods:{
            createPost(){
                this.post.id= Date.now();
                this.$emit('create',this.post);
                this.post={
                    title:'',
                    body:''
                }
            }
        }
    }
</script>

<style scoped>

    button{
        background-color: darkslategray;
        color: aliceblue;
        outline: none;
        border: none;
        cursor: pointer;
        padding: 10px;
        margin: 5px;
    }
    form{
        display: flex;
        flex-direction: column;
    }
</style>