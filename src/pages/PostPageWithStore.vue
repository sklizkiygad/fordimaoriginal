<template>
    <div>

<!--        <h1>{{$store.state.post.limit}}</h1>-->
<!--        <h1>{{$store.state.post.posts}}</h1>-->

        <h1>Страница с Димами </h1>


        <my-input

               :model-value="searchQuery"
               @update:model-value="setSearchQuery"
                placeholder="Поиск..."/>

        <div class="app_btns">

            <my-button
                    @click="showDialog"
            >
                Создать Кабачок</my-button>
            <my-select
                    :model-value="selectedSort"

                  @update:model-value="setSelectedSort"
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
    import MyButton from "../components/UI/MyButton";
    import {mapState,mapGetters,mapMutations,mapActions} from 'vuex';

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


            }
        },
        methods:{

            ...mapMutations({
                setPage:'post/setPage',
                setSearchQuery:'post/setSearchQuery',
                setSelectedSort:'post/setSelectedSort'


            }),
            ...mapActions({
                loadMorePosts:'post/loadMorePosts',
                fetchPosts:'post/fetchPosts',

            }),


            createPost(post){
                this.posts.push(post);
                this.dialogVisible=false;

            },
            removePost(post){
                //this.posts=this.posts.filter(p=>p.id !== post.id)
                this.$store.state.post.posts=this.$store.state.post.posts.filter(p=>p.id !== post.id)

               console.log(this.$store.state.post.posts);

            },
            showDialog(){
                this.dialogVisible=true;
            },
           /* changePage(pageNumber){
                this.page=pageNumber;
                this.fetchPosts();

            },*/
            // async fetchPosts(){
            //     try{
            //         this.isPostsLoading=true;
            //
            //         const response=await axios.get('https://jsonplaceholder.typicode.com/posts',{
            //             params:{
            //                 _page:this.page,
            //                 _limit:this.limit
            //             }
            //         });
            //         this.totalPages = Math.ceil(response.headers['x-total-count']/this.limit)
            //         this.posts=response.data;
            //     }
            //     catch (e) {
            //         alert('Ошибка');
            //
            //     }
            //     finally {
            //         this.isPostsLoading=false;
            //
            //     }
            //
            // },
            //
            // async loadMorePosts(){
            //     try{
            //         this.page+=1;
            //
            //
            //         const response=await axios.get('https://jsonplaceholder.typicode.com/posts',{
            //             params:{
            //                 _page:this.page,
            //                 _limit:this.limit
            //             }
            //         });
            //         this.totalPages = Math.ceil(response.headers['x-total-count']/this.limit)
            //         this.posts=[...this.posts,...response.data];
            //     }
            //     catch (e) {
            //         alert('Ошибка');
            //
            //     }
            //
            //
            // },


        },
        mounted(){
            this.fetchPosts();
          //  this.fetchPosts();
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

            ...mapState({
                posts: state=>state.post.posts,

                isPostsLoading:state=>state.post.isPostsLoading,
                searchQuery:state=>state.post.searchQuery,
                selectedSort:state=>state.post.selectedSort,
                page:state=>state.post.page,
                limit:state=>state.post.limit,
                totalPages:state=>state.post.totalPages,
                sortOptions:state=>state.post.sortOptions,


            }),
            ...mapGetters({
                sortedPosts: 'post/sortedPosts',
                sortedAndSearchedPosts: 'post/sortedAndSearchedPosts',


            })



            // sortedPosts(){
            //     return [...this.posts].sort((post1,post2)=> post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]));
            // },
            // sortedAndSearchedPosts(){
            //     return this.sortedPosts.filter(post=>post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
            // }

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
