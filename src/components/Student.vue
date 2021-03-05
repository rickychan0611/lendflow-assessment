<template>
  <div class="list-container">
    <div class="pic-container">
      <img class="picture" :src="student.pic" />
    </div>

    <div class="student-content">
      <h1 class="full-name">
        {{ student.firstName + " " + student.lastName }}
      </h1>
      <ul class="info">
        <li>Email: {{ student.email }}</li>
        <li>Company: {{ student.company }}</li>
        <li>Skil: {{ student.skill }}</li>
        <li>Average: {{ getAverage }}%</li>
      </ul>

      <table class="grade-table" v-if="showGrades">
        <tr v-for="(grade, index) in grades" :key="grade">
          <td class="test-num">Test {{ index }}</td>
          <td class="grade">{{ grade }}%</td>
        </tr>
      </table>

      <div class="tags-container" v-if="student.tags">
        <div class="tag-box" v-for="tag in student.tags" :key="tag">
          {{ tag }}
        </div>
        <br />
      </div>
      <input
        class="new-tag-input"
        placeholder="Add a tag"
        @change="newTagValue"
        v-model="tag"
      />
    </div>

    <div class="expand-button" @click="toggleGrades">
      {{ showGrades ? "-" : "+" }}
    </div>
  </div>
</template>

<script>
export default {
  props: ["student"],
  data() {
    return {
      grades: this.student.grades,
      showGrades: false,
      tags: this.student.tags ? this.student.tags : [],
      newTag: "",
      tag: "",
    };
  },
  computed: {
    getAverage: function () {
      return (
        this.student.grades.reduce((a, b) => +a + +b) /
        this.student.grades.length
      );
    },
  },
  methods: {
    toggleGrades() {
      this.showGrades = !this.showGrades;
    },
    newTagValue({ target }) {
      this.newTag = target.value;
      this.tags.push(this.newTag);
      this.$emit("update-tags", { id: this.student.id, tags: this.tags });
      this.tag = "";
    },
  },
};
</script>

<style scoped>
.list-container {
  display: grid;
  grid-template-columns:
    minmax(100px, 100px)
    minmax(auto, 800px)
    minmax(50px, 50px);
  grid-gap: 30px;
  justify-content: center;
  border-style: solid;
  border-color: lightgray;
  border-width: 0 0 1px 0;
  margin-bottom: 10px;
}
.pic-container {
  display: grid;
  justify-content: center;
  margin-top: 10px;
}
.picture {
  border-radius: 50%;
  border: 1px solid lightgrey;
  width: 100px;
}
.full-name {
  text-transform: uppercase;
  font-weight: 700;
  margin: 0px;
}
.info {
  list-style-type: none;
  margin-left: -20px;
  color: grey;
  font-size: 14px;
  margin-bottom: 0px;
}
.info li {
  margin: 5px 0;
}
.grade-list {
  list-style-type: none;
  counter-reset: elementcounter;
  padding-left: 0;
}
.grade-table {
  margin-left: 17px;
  margin-bottom: 17px;
}
.grade-table tr {
  font-family: Raleway, sans-serif;
  color: grey;
  font-size: 14px;
  margin-left: 20px;
}
.test-num {
  width: 60px;
}
.expand-button {
  display: grid;
  height: 20%;
  justify-content: end;
  font-size: 80px;
  color: grey;
  margin-top: -28px;
}
.new-tag-input {
  border: 0px;
  outline: none;
  font-family: Raleway, sans-serif;
  font-size: 15px;
  background: transparent;
  width: 200px;
  margin: 10px 0 20px 16px;
  border-bottom: 1px solid lightgray;
  padding: 10px;
}
.new-tag-input:focus {
  border-bottom: 2px solid gray;
}
.new-tag-input::placeholder {
  font-family: Raleway, sans-serif;
  font-size: 15px;
}
.tags-container {
  margin-left: 16px;
}
.tag-box {
  display: inline-block;
  background-color: #dbdbdb;
  padding: 10px;
  border-radius: 5px;
  margin-top: 10px;
  margin-right: 5px;
}
</style>
