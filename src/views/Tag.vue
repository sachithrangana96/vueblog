<template>

    <div class="tag">

        <div v-if="error">{{ error}}</div>

        <div v-if="posts.length" class="layout">
            <PsotList  :posts="postsWithTag" />
            <TagCloud :posts="posts" />
        </div>
    
        <div v-else>
        
            <Spinner />

        </div>

    </div>


</template>

<script>

import PsotList from '../components/PostList.vue'
import getPosts from '../composable/getPosts'
import TagCloud from '../components/TagCloud.vue'
import Spinner from '../components/Spinner'
import {useRoute} from 'vue-router'
import {computed} from 'vue'


export default {
    components:{
        PsotList,Spinner,TagCloud
    },

    setup(props) {

        const route = useRoute()
        const { posts,error ,load } = getPosts()

        load()

        const postsWithTag = computed(()=> {
            return posts.value.filter((p) => p.tags.includes(route.params.tag))
        })

        return { error,posts,postsWithTag}
    }

}
</script>