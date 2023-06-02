<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>
      <p v-if="isLoading">Loading...</p>
      <p v-else-if="!isLoading && error">{{ error }}</p>
      <p v-else-if="!isLoading && (!results || results.length === 0)">
        No experiences found start adding!
      </p>
      <ul v-else>
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';
import axios from 'axios';
export default {
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      isLoading: false,
      error: null,
    };
  },
  methods: {
    async loadExperiences() {
      this.isLoading = true;
      this.error = null;
        try {
          const response = await axios.get(
        'https://vue-http-demo-3b7aa-default-rtdb.firebaseio.com/surveys.json'
      );
      const data = await response.data;
      this.results = data;
      console.log(data);
      this.isLoading = false;
        } catch (error) {
          // this.isLoading = false;
          console.log(`Error: ${error}`);
          this.error = 'Failed to fetch data - try again later';
          this.isLoading = false;
        }
   
    
    },
  },
  mounted() {
    this.loadExperiences();
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
