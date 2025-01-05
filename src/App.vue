<script setup>
import Navbar from "@/components/Navbar.vue";
import Hero from "@/components/Hero.vue";
import HomeCard from "@/components/HomeCard.vue";
import JobListing from "@/components/JobListings.vue";
</script>
<template>
  <Navbar />
  <Hero title="Tharindu" subTitle="Hey there welcome to my blog" />
  <HomeCard />
  <JobListing :limits="3" :showButton="true" />
</template>
