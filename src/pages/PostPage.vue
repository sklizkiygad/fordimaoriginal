<template>
    <div>

        <h1>Страница с Димами </h1>

        <my-input
                v-focus
                v-model="searchQuery"
                placeholder="Поиск..."/>

        <div class="app_btns">

            <my-button
                    @click="showDialog"
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
                    @create="createPost"
            />

        </my-dialog>

        <post-list
                :posts="sortedAndSearchedPosts"
                @remove="removePost"
                v-if="!isPostsLoading"
        />
        <div v-else>Посты загружаются...</div>
        <div v-intersection="loadMorePosts" class="observer"></div>
<!--        <div class="page_wrapper">-->
<!--            <div-->
<!--                    v-for="pageNumber in totalPages"-->
<!--                    :key="pageNumber"-->
<!--                    class="page"-->
<!--                    :class="{'current_page':page===pageNumber}"-->
<!--                    @click="changePage(pageNumber)">-->
<!--                {{pageNumber}}-->
<!--            </div>-->

<!--        </div>-->
    </div>
</template>

<script>
    import PostForm from "@/components/PostForm";
    import PostList from "@/components/PostList";
    import MyDialog from "@/components/UI/MyDialog";
    import MyInput from "@/components/UI/MyInput";

    import axios from "axios";

    export default {
        components:{
            MyInput,
            MyDialog,
            PostList,
            PostForm,

        },
        data(){
            return{
                posts:[],
                dialogVisible:false,
                isPostsLoading:false,
                searchQuery:'',
                selectedSort:'',
                page:1,
                limit:10,
                totalPages:0,
                sortOptions:[
                    {value:'title',name:'По названию'},
                    {value:'body',name:'По описанию'}
                ]

            }
        },
        methods:{
            createPost(post){
                this.posts.push(post);
                this.dialogVisible=false;

            },
            removePost(post){
                this.posts=this.posts.filter(p=>p.id !== post.id)


            },
            showDialog(){
                this.dialogVisible=true;
            },
           /* changePage(pageNumber){
                this.page=pageNumber;
                this.fetchPosts();

            },*/
            async fetchPosts(){
                try{
                    this.isPostsLoading=true;

                    const response=await axios.get('https://jsonplaceholder.typicode.com/posts',{
                        params:{
                            _page:this.page,
                            _limit:this.limit
                        }
                    });
                    this.totalPages = Math.ceil(response.headers['x-total-count']/this.limit)
                    this.posts=response.data;
                }
                catch (e) {
                    alert('Ошибка');

                }
                finally {
                    this.isPostsLoading=false;

                }

            },

            async loadMorePosts(){
                try{
                    this.page+=1;


                    const response=await axios.get('https://jsonplaceholder.typicode.com/posts',{
                        params:{
                            _page:this.page,
                            _limit:this.limit
                        }
                    });
                    this.totalPages = Math.ceil(response.headers['x-total-count']/this.limit)
                    this.posts=[...this.posts,...response.data];
                }
                catch (e) {
                    alert('Ошибка');

                }


            },


        },
        mounted(){
            this.fetchPosts();
            // const obs=this.$refs.observer;
            // console.log(obs);
            // const options = {
            //     rootMargin: '0px',
            //     threshold: 1.0
            // }
            // const callback=(entries,observer)=>{
            //     if(entries[0].isIntersecting && this.page < this.totalPages){
            //        this.loadMorePosts();
            //
            //     }
            //
            // }
            //
            // const observer = new IntersectionObserver(callback, options);
            // observer.observe(obs)

        },
        computed:{
            sortedPosts(){
                return [...this.posts].sort((post1,post2)=> post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]));
            },
            sortedAndSearchedPosts(){
                return this.sortedPosts.filter(post=>post.title.toLowerCase().includes(this.searchQuery.toLowerCase()));
            }

        },
        watch:{

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
