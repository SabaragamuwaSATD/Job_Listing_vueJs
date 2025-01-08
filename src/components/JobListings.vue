<script setup>
import { ref, onMounted, reactive } from "vue";
import JobListing from "@/components/JobListing.vue";
import { RouterLink } from "vue-router";
import PulseLoader from "vue-spinner/src/PulseLoader.vue";
import axios from "axios";

const props = defineProps({
  limits: Number,
  showButton: {
    type: Boolean,
    default: false,
  },
});

const state = reactive({
  jobs: [],
  isLoading: true,
});

onMounted(async () => {
  try {
    const response = await axios.get("/api/jobs");
    state.jobs = response.data;
  } catch (error) {
    console.error(error);
  } finally {
    state.isLoading = false;
  }
});
</script>

<template>
  <section class="bg-blue-50 px-4 py-10">
    <div class="container-xl lg:container m-auto">
      <h2 class="text-3xl font-bold text-green-500 pb-6">Browse Jobs</h2>
      <!--Show loading spinner while loading is true-->
      <div v-if="state.isLoading" class="text-center text-gray-500 py-6">
        <PulseLoader color="#10B981" />
      </div>

      <!--Show job listings when loading is false-->
      <div v-else class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <JobListing
          v-for="job in state.jobs.slice(0, props.limits || state.jobs.length)"
          :key="job.id"
          :job="job"
          @log-job="(job) => console.log('Job passed:', job)"
        />
      </div>
    </div>
  </section>
  <section v-if="props.showButton" class="m-auto max-w-lg my-10 px-6">
    <RouterLink
      to="/jobs"
      class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
      >View All Jobs</RouterLink
    >
  </section>
</template>
