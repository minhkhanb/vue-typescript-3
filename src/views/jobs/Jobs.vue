<template>
  <div id="tuts">
    <div v-for="job in jobs" :key="job.id" class="job">
      <router-link :to="{ name: 'JobDetails', params: { id: job.id } }">
        <h2>{{ job.title }}</h2>
      </router-link>
    </div>

    <div class="positions" v-if="positions.length">
      <h2>Position</h2>
      <div class="pos" v-for="pos in positions" :key="pos.id">
        <p>{{ pos.title }}</p>
      </div>
    </div>
    <div v-else>
      <p>Loading positions...</p>
    </div>

    <div class="job-app">
      <header>
        <h1>
          <img src="../../assets/logo.png" alt="" />
        </h1>
        <div class="order">
          <button @click="handleClick('title')">order by title</button>
          <button @click="handleClick('salary')">order by salary</button>
          <button @click="handleClick('location')">order by location</button>
        </div>
      </header>
      <JobList :jobs="jobs" :order="order" />
    </div>
    <!-- <nav class="navbar navbar-expand navbar-dark bg-dark">
      <router-link to="/" class="navbar-brand">bezKoder</router-link>
      <div class="navbar-nav mr-auto">
        <li class="nav-item">
          <router-link to="/tutorials" class="nav-link">Tutorials</router-link>
        </li>
        <li class="nav-item">
          <router-link to="/add" class="nav-link">Add</router-link>
        </li>
      </div>
    </nav> -->

    <div class="container mt-3">
      <router-view />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import JobList from '@/components/JobsList.vue';
import { Job } from '@/types/Job';
import { OrderTerm } from '@/types/OrderTerm';

export default defineComponent({
  name: 'Jobs',
  components: {
    JobList,
  },
  data() {
    const positions: Job[] = [];

    return {
      positions,
    };
  },
  setup() {
    const jobs = ref<Job[]>([
      {
        title: 'farm worker',
        location: 'lon lon ranch',
        salary: 1000,
        id: '1',
      },
      {
        title: 'quarryman',
        location: 'death mountain',
        salary: 40000,
        id: '2',
      },
      {
        title: 'flute player',
        location: 'the lost woods',
        salary: 35000,
        id: '3',
      },
      {
        title: 'fisherman',
        location: 'lake hylia',
        salary: 21000,
        id: '4',
      },
      {
        title: 'prison guard',
        location: 'gerudo valley',
        salary: 32000,
        id: '5',
      },
    ]);

    const order = ref<OrderTerm>('title');

    const handleClick = (term: OrderTerm) => {
      order.value = term;
    };

    return {
      jobs,
      handleClick,
      order,
    };
  },
  mounted() {
    fetch('http://localhost:3000/jobs')
      .then((res) => res.json())
      .then((data) => (this.positions = data))
      .catch((err) => console.log(err.message));
  },
});
</script>

<style>
#tuts {
  text-align: center;
}

.job h2 {
  background: #ddd;
  padding: 20px;
  border-radius: 10px;
  margin: 10px auto;
  max-width: 600px;
  cursor: pointer;
  color: #444;
}

.job h2:hover {
  background: #ccc;
}

.job a {
  text-decoration: none;
}
</style>
