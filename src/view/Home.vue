<template>
    <div class="w-full flex flex-col gap-5 p-5">
        <p>This repo belongs to the user, <b>{{ repoUser }}</b>.</p>
        <p>*The provided instruction asks for 'title', 'description', and 'url of repo', but the data doesn't contain 'title' so I use 'name' instead.</p>
        <div 
        v-for="(repo, repoIndex) in repos"
        :key="repoIndex"
        class="p-5 bg-blue-100 flex flex-col gap-3"
        style="word-break: break-word;"
        >
            <span><b>Name:</b> {{ repo?.name }}</span>
            <span><b>Description:</b> {{ repo?.description }}</span>
            <span><b>Url:</b> <a :href="repo?.html_url" target="_blank" class="text-blue-500">{{ repo?.html_url }}</a></span>
        </div>
        <Loader v-if="pagination.hasMore" @intersect="getData" />
    </div>
</template>

<script setup>
    import Loader from '@/components/Loader.vue';
    import { onMounted, reactive, ref } from 'vue';

    const pagination = reactive({
        pageSize: 10,
        currentPage: 0,
        hasMore: true,
        isLoading: false
    })

    const repos = ref([])
    let repoUser = 'defunkt'

    const getData = async() =>{
        if(pagination.isLoading || !pagination.hasMore) return
        pagination.isLoading = true

        try{
            pagination.currentPage++
            const res = await fetch(`https://api.github.com/users/${repoUser}/repos?page=${pagination.currentPage}&per_page=${pagination.pageSize}`)
            const data = await res.json()
            if(data?.length > 0){
                repos.value = repos.value.concat(data)
            }
            else{
                pagination.hasMore = false
            }
        }
        catch(err){
            console.log(err, 'error')
        }
        finally{
            pagination.isLoading = false
        }
    }

    onMounted(() =>{
        getData()
    })
</script>