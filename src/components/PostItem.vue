<template>
    <div class="post">
        <h4>{{post.id}}</h4>
        <div>
            <div>Название: {{post.title}}</div>
            <div>Описание: {{post.body}}</div>
        </div>

       <div class="post_btns">
           <my-button @click="$router.push(`/posts/${post.id}`)">Открыть</my-button>
           <my-button @click="$emit('remove', post)">Удолить</my-button>

       </div>



    </div>
</template>

<script>


    import MyButton from "./UI/MyButton";
    export default {
        components: {MyButton},
        props:{
            post:{
                type:Object,
                required:true

            }
        }

    }
</script>

<style scoped>
    .post{
        padding: 15px;
        border:2px solid darkslategray;
        margin-top: 15px;
    }
    .post_btns{
        text-align: right;
        position: relative;
        top:-45px;
    }

</style>