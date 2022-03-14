<template>
    <div>


        <h1>Страница с Димами </h1>

        <my-input
                v-focus
                v-model="searchQuery"
                placeholder="Поиск..."/>

        <div class="app_btns">

            <my-button

            >
                Создать Кабачок</my-button>
            <my-select
                    v-model="selectedSort"
                    :options="sortOptions"
            ></my-select>

        </div>




        <my-dialog
                v-model:show="dialogVisible"
        >
            <PostForm

            />

        </my-dialog>

        <post-list
                :posts="SortedAndSearchedPosts"

                v-if="!isPostsLoading"
        />
        <div v-else>Посты загружаются...</div>
        <div v-intersection="posts" class="observer"></div>

    </div>
</template>

<script>
    import PostForm from "@/components/PostForm";
    import PostList from "@/components/PostList";
    import MyDialog from "@/components/UI/MyDialog";
    import MyInput from "@/components/UI/MyInput";

    import axios from "axios";
    import {ref} from 'vue';
    import MyButton from "../components/UI/MyButton";
    import {usePosts} from "../hooks/usePosts";
    import {useSortedPosts} from "../hooks/useSortedPosts";
    import {useSortedAndSearchedPosts} from "../hooks/useSortedAndSearchedPosts";

    export default {
        components:{
            MyButton,
            MyInput,
            MyDialog,
            PostList,
            PostForm,

        },
        data(){
            return{

                dialogVisible:false,

                sortOptions:[
                    {value:'title',name:'По названию'},
                    {value:'body',name:'По описанию'}
                ]

            }
        },
     setup(props){
       const {posts,isPostsLoading,totalPages}=usePosts(10);

       const {sortedPosts,selectedSort}=useSortedPosts(posts);
       const {searchQuery,SortedAndSearchedPosts}=useSortedAndSearchedPosts(sortedPosts);
         return{

             posts,
             totalPages,
             isPostsLoading,
             sortedPosts,
             selectedSort,
             searchQuery,
             SortedAndSearchedPosts
         }
     }










    }


</script>

<style>

    .app_btns{
        display:flex;
        justify-content: space-between;
    }
    .page_wrapper{
        display: flex;
        margin-top: 15px;

    }
    .page{
        border: 1px solid darkblue;
        padding: 10px;
        cursor:pointer;
    }
    .page:hover{
        color: aliceblue;
        background-color: darkblue;

    }
    .current_page{
        border:1px solid red;
        color: aliceblue;
        background-color: red;
    }
    .current_page:hover{
        border:1px solid red;
        color: aliceblue;
        background-color: red;
    }
    .observer{
        height: 30px;
        /*background-color: green;*/
    }
</style>
