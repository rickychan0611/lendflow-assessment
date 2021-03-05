<template>
  <Card :data="studentsData" />
</template>

<script>
import Card from "./components/Card.vue";

export default {
  components: {
    Card,
  },
  data() {
    return {
      studentsData: [],
    };
  },
  created() {
    this.getData();
  },
  methods: {
    getData() {
      fetch("https://api.hatchways.io/assessment/students")
        .then((response) => {
          if (response.ok) {
            return response.json();
          }
        })
        .then((data) => {
          this.studentsData = data.students;
        });
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Raleway:wght@400;700&display=swap");

body {
  background-color: lightgray;
  display: flex;
  justify-content: center;
  align-items: center;
}
#app {
  font-family: Raleway, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translateX(-50%) translateY(-50%);
}
</style>
