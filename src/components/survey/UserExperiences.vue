<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences">Load Submitted Experiences</base-button>
      </div>
      <p v-if="isError != ''">{{ isError }}</p>
      <!-- <p v-if="isLoading && isError === ''">Loading...</p> -->

      <div v-if="isLoading && isError === ''" class="lds-spinner">
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
      </div>

      <p style="color: red" v-else-if="!isLoading && (!results || results.length === 0)">
        No stored experiences found. Stat adding some survey results first.
      </p>
      <ul v-else>
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
          :test="result.test"
        >
          <base-button @click="deleteExperience(result.id)">Delete</base-button></survey-result
        >
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue'

export default {
  // props: ['results'],
  components: {
    SurveyResult
  },
  data() {
    return {
      results: [],
      isLoading: false,
      isError: ''
    }
  },
  methods: {
    loadExperiences() {
      this.isLoading = true
      fetch('https://vue-http-demo-b6333-default-rtdb.firebaseio.com/surveys.json')
        .then((response) => {
          if (response.ok) {
            return response.json()
            // console.log(response.json())
          }
        })
        .then((data) => {
          this.isLoading = false
          const results = []
          for (const id in data) {
            // console.log(id)

            results.push({ id: id, name: data[id].name, rating: data[id].rating })
          }
          this.results = results
        })
        .catch((error) => {
          if (error) {
            this.isError = 'Failed to fetch data - please try again!'
          }
        })
    },
    deleteExperience(id) {
      console.log(id)

      fetch(`https://vue-http-demo-b6333-default-rtdb.firebaseio.com/surveys/${id}.json`, {
        method: 'DELETE',
        headers: { 'Content-Type': 'application/json' }
      })
        .then((response) => {
          if (response.ok) {
            this.loadExperiences()
          } else {
            alert('Something went wrong - please try again!')
          }
        })
        .catch((error) => {
          console.log(error)
        })
    }
  },
  mounted() {
    this.loadExperiences()
    console.log('mted')
  }
}
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

.lds-spinner,
.lds-spinner div,
.lds-spinner div:after {
  box-sizing: border-box;
}
.lds-spinner {
  color: currentColor;
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
}
.lds-spinner div {
  transform-origin: 40px 40px;
  animation: lds-spinner 1.2s linear infinite;
}
.lds-spinner div:after {
  content: ' ';
  display: block;
  position: absolute;
  top: 3.2px;
  left: 36.8px;
  width: 6.4px;
  height: 17.6px;
  border-radius: 20%;
  background: currentColor;
}
.lds-spinner div:nth-child(1) {
  transform: rotate(0deg);
  animation-delay: -1.1s;
}
.lds-spinner div:nth-child(2) {
  transform: rotate(30deg);
  animation-delay: -1s;
}
.lds-spinner div:nth-child(3) {
  transform: rotate(60deg);
  animation-delay: -0.9s;
}
.lds-spinner div:nth-child(4) {
  transform: rotate(90deg);
  animation-delay: -0.8s;
}
.lds-spinner div:nth-child(5) {
  transform: rotate(120deg);
  animation-delay: -0.7s;
}
.lds-spinner div:nth-child(6) {
  transform: rotate(150deg);
  animation-delay: -0.6s;
}
.lds-spinner div:nth-child(7) {
  transform: rotate(180deg);
  animation-delay: -0.5s;
}
.lds-spinner div:nth-child(8) {
  transform: rotate(210deg);
  animation-delay: -0.4s;
}
.lds-spinner div:nth-child(9) {
  transform: rotate(240deg);
  animation-delay: -0.3s;
}
.lds-spinner div:nth-child(10) {
  transform: rotate(270deg);
  animation-delay: -0.2s;
}
.lds-spinner div:nth-child(11) {
  transform: rotate(300deg);
  animation-delay: -0.1s;
}
.lds-spinner div:nth-child(12) {
  transform: rotate(330deg);
  animation-delay: 0s;
}
@keyframes lds-spinner {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}
</style>
