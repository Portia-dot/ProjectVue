<template>
  <div>
    <b-jumbotron class="container">
      <div class="studentprofile ">
        <div class="searchbar">
          <input
            class="search"
            v-model="search"
            type="text"
            placeholder="Search by name"
          />
          <input
            class="search2"
            type="text"
            placeholder="Search By Tag"
            v-model="searchTag"
          />
        </div>
        <div
          class="content d-flex"
          v-for="student in studentList"
          :key="student.id"
        >
          <div class="img">
            <img :src="student.pic" alt="someiame" />
          </div>
          <div class="details">
            <h3>{{ student.firstName + " " + student.lastName }}</h3>
            <h5>{{ student.company }}</h5>
            <h5>{{ student.skill }}</h5>
            <h5>{{ student.email }}</h5>
            <h5>Average Grade: {{ averageGrade(student) }}</h5>
            <button v-on:click="isHidden = !isHidden">
              <i class="fas fa-plus"> </i>
            </button>
            <div class="test" v-if="!isHidden">
              <p>Test 1: {{ student.grades[0] }}</p>
              <p>Test 2: {{ student.grades[1] }}</p>
              <p>Test 3: {{ student.grades[2] }}</p>
              <p>Test 5: {{ student.grades[3] }}</p>
              <p>Test 5: {{ student.grades[4] }}</p>
              <p>Test 6: {{ student.grades[5] }}</p>
              <p>Test 7: {{ student.grades[6] }}</p>
              <p>Test 8: {{ student.grades[7] }}</p>
            </div>
            <div
              class="input wrapper flex items-center"
              v-for="(input, index) in tags"
              :key="
                `phoneInp
              ut-${index}`
              "
            >
              <input
                v-model="input.nameTag"
                type="text"
                class="h-10 rounded-lg outline-none p-2"
                placeholder=" Enter Phone Number"
                @keyup.enter="addField(input, tags)"
              />
            </div>
            <!-- <ul>
              <li>
                {{ input }}
              </li>
            </ul>
            <div
              v-for="(input, index) in tagsNumber"
              :key="`phoneInput- ${index}`"
            >
              <input
                v-model="input.phone"
                type="text"
                placeholder="Add a Tag"
                @keyup.enter="addField(input, tagsNumber)"
              />
            </div> -->
          </div>
        </div>
      </div>
      <hr />
    </b-jumbotron>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      tags: [{ nameTag: "" }],
      students: null,
      search: "",
      isHidden: true,
      // todos: [""],
      todoModel: "",
      searchTag: ""
    };
  },
  created() {
    axios
      .get("https://api.hatchways.io/assessment/students")
      .then(response => {
        this.students = response.data.students;
        console.log(response.data.students);
      })
      .catch(error => {
        console.log("there is an error:" + error.response);
      });
  },
  computed: {
    studentList() {
      if (this.search.trim().length > 0) {
        return this.students.filter(item => {
          return this.search
            .toLowerCase()
            .split(" ")
            .some(v => item.firstName.toLowerCase().includes(v));
        });
      } else {
        return this.students;
      }
    }
  },
  methods: {
    averageGrade(student) {
      return (
        student.grades.reduce((total, grade) => (total += parseInt(grade)), 0) /
        student.grades.length
      );
    },
    addField(value, fieldType) {
      fieldType.push({ value: "" });
    },
    storeTodo() {
      this.todos.push(this.todoModel);
    }
  }
};
</script>

<style scoped>
.container {
  overflow-y: auto;
  max-height: 80vh;
}
.form-inline {
  justify-content: center;
}
.searchbar {
  text-align: center;
  margin-bottom: 30px;
}
.studentprofile {
  border-bottom: 1px solid #dcdcdc;
}
img {
  max-width: 100px;
  height: 100px;
  border-radius: 50%;
  border: 1px solid black;
  margin-right: 20px;
  margin-top: 15px;
}
input {
  width: 90%;
  padding: 10px 8px;
  margin: 5px 0;
  border: none;
  border-bottom: 2px solid gray;
}
.content {
  margin: 10px 20px;
  border-bottom: 1px solid #dcdcdc;
}
hr {
  border: 1x solid gray;
}
button {
  position: relative;
  left: 30rem;
}
.test,
.tagsearch {
  display: block;
}
ul {
  text-decoration: none;
  list-style-type: none;
}
@media only screen and (max-width: 736px) {
  button {
    position: absolute;
    right: 20px;
  }
}
@media only screen and (max-width: 820px) {
  button {
    position: absolute;
    right: 20px;
  }
}
</style>
