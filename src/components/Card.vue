<template>
  <div class="card">
    <div class="search-box-container">
      <input
        class="search-box"
        placeholder="Search by name"
        @input="onSearch"
        v-model="nameSearchValue"
      />
      <hr class="input-line" />
      <input
        class="search-box"
        placeholder="Search by tag"
        @input="onSearch"
        v-model="tagSearchValue"
      />
      <hr />
    </div>

    <div
      class="student-list"
      v-for="student in filteredStudents"
      :key="student.id"
    >
      <Student :student="student" @update-tags="updateTags" />
    </div>
  </div>
</template>

<script>
import Student from "./Student.vue";

export default {
  components: {
    Student,
  },
  props: ["data"],
  data() {
    return {
      searchValue: "",
      studentData: [],
      filteredStudents: [],
      nameSearchValue: "",
      tagSearchValue: "",
    };
  },
  watch: {
    data: function () {
      this.studentData = this.data;
      this.filteredStudents = this.studentData;
    },
  },
  methods: {
    onSearch() {
      if (
        (this.nameSearchValue === null || this.nameSearchValue === undefined) &&
        (this.tagSearchValue === null || this.tagSearchValue === undefined)
      ) {
        this.filteredStudents = this.data;
      } else {
        this.filteredStudents = this.studentData.filter((student) => {
          const fullName = (
            student.firstName +
            " " +
            student.lastName
          ).toLowerCase();
          const allTags = student.tags
            ? student.tags.join(" ").toLowerCase()
            : "";

          if (
            fullName.includes(this.nameSearchValue.toLowerCase()) &&
            allTags.includes(this.tagSearchValue.toLowerCase())
          ) {
            return true;
          } else false;
        });
      }
    },
    updateTags(event) {
      this.studentData.map((student, index) => {
        if (student.id === event.id) {
          this.studentData[index].tags = event.tags;
        }
      });
    },
  },
};
</script>

<style scoped>
.card {
  position: relative;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  border-radius: 8px;
  background-color: white;
  overflow: scroll;
  max-height: 80vh;
  max-width: 90vw;
  min-width: 600px;
}
.search-box-container {
  background-color: white;
  position: -webkit-sticky;
  position: sticky;
  top: 0px;
  padding: 20px 10px 0px 10px;
}
.search-box {
  border: 0px;
  outline: none;
  font-family: Raleway, sans-serif;
  font-size: 15px;
  background: transparent;
  width: 100%;
}
.search-box::placeholder {
  font-family: Raleway, sans-serif;
  font-size: 15px;
}
.input-line {
  margin-bottom: 20px;
}
hr {
  border-top: 1px solid lightgray;
  /* margin-bottom: 20px; */
}
.student-list {
  margin: 20px 20px 0 20px;
}
</style>
