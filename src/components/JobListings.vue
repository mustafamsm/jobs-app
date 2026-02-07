<template>
    <section class="bg-gray-50 px-4 py-10">
        <div class="container-xl lg:container m-auto">
            <h2 class="text-3xl font-bold text-center mb-6 text-green-500">Browse Jobs</h2>
            <!-- show loading spinner while load data -->
            <div v-if="state.isLoading" class="text-center text-gray-500 py-6">
                <PulseLoader />
             </div>
            <!-- show job listing when done loading -->
            <div v-else class="grid grid-cols-1 gap-6 md:grid-cols-3 ">
                <JobListing v-for="job in state.jobs.slice(0, limit || state.jobs.length)" :key="job.id" :job="job" />

            </div>
        </div>

    </section>
    <section v-if="showButton" class="m-auto max-w-lg my-10 px-6">
        <RouterLink to="/jobs" class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700">View
            All Jobs</RouterLink>
    </section>

</template>

<script setup>
import { reactive, defineProps, onMounted } from 'vue'
import JobListing from './JobListing.vue'
import { RouterLink } from 'vue-router';
import PulseLoader from 'vue-spinner/src/PulseLoader.vue'
defineProps({
    limit: Number,
    showButton: {
        type: Boolean,
        default: false,
    }
})
const state = reactive({
    jobs: [],
    isLoading: true
})
onMounted(async () => {
    try {
        const response = await fetch('http://localhost:3000/jobs')
            .then((res) => res.json())
            .then(data => {
                state.jobs = data
            })

    } catch (error) {
        console.error('Error fwtching jobs', error)
    } finally {
        state.isLoading = false
    }
})

</script>

<style lang="scss" scoped></style>