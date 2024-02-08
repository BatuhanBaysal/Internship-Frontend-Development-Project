<template>
  <div id="app">
    <h2>Courses</h2>
    <ul>
      <li v-for="(course, index) in courses" class="list" :class="{selected: course.selected}" @click="selectCourse(course)">
        {{ course.name }} | Credit: {{ course.credit }} | Grade: {{ course.grade }}
        <button @click="removeCourse(index)">
          <span class="material-symbols-outlined">❌</span> 
        </button>
      </li>
    </ul>
    <h3>List Courses</h3>
    <ul>
      <li v-for="selectedCourse in selectedCourses">
        {{ selectedCourse.name }} | Credit: {{ selectedCourse.credit }} | Grade: {{ selectedCourse.grade }}
      </li>
    </ul>
    <div>
      <input type="text" v-model="newCourseName" placeholder="Course Name" class="input-field">
      <input type="number" v-model.number="newCourseCredit" placeholder="Credit" @input="checkCreditInput" class="input-field">
      <input type="text" v-model="newCourseGrade" placeholder="Grade (letter)" @input="checkGradeInput" class="input-field">
      <button @click="addCourse" class="button">Add Course</button>
      <p v-if="creditInputError">{{ creditInputError }}</p>
      <p v-if="gradeInputError">{{ gradeInputError }}</p> 
    </div>
    <h3>GPA: {{ gpa }}</h3>
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      courses: [],
      newCourseName: '',
      newCourseCredit: '',
      newCourseGrade: '',
      gradeInputError: '' 
    }
  },
  methods: {
    selectCourse(course) {
      course.selected = !course.selected;
    },
    addCourse() {
      if (this.newCourseName.trim() === '') {
        this.creditInputError = 'Course Name cannot be empty.';
        return;
      }
      if (this.newCourseCredit === '' || isNaN(this.newCourseCredit) || this.newCourseCredit < 0 || this.newCourseCredit > 30) {
        this.creditInputError = 'Invalid credit value. Credit must be between 1 and 30.';
        return; 
      }
      if (this.newCourseGrade.trim() === '') {
        this.gradeInputError = 'Grade cannot be empty.';
        return; 
      }
      const validGrades = ['A', 'A-', 'B+', 'B', 'B-', 'C+', 'C', 'C-', 'D+', 'D', 'D-', 'F'];
      if (!validGrades.includes(this.newCourseGrade.toUpperCase())) {
        this.gradeInputError = 'Invalid grade.';
        return; 
      }
  
      this.courses.push({
        name: this.newCourseName,
        credit: parseInt(this.newCourseCredit),
        grade: this.newCourseGrade.toUpperCase(), 
        selected: false
      });
      this.newCourseName = '';
      this.newCourseCredit = '';
      this.newCourseGrade = '';
      this.creditInputError = '';
      this.gradeInputError = '';
    },
    removeCourse(index) {
      this.courses.splice(index, 1);
    },
    checkGradeInput() {
      const validGrades = ['A', 'A-', 'B+', 'B', 'B-', 'C+', 'C', 'C-', 'D+', 'D', 'D-', 'F'];
      if (!validGrades.includes(this.newCourseGrade.toUpperCase())) {
        this.gradeInputError = 'Invalid grade.';
      } else {
        this.gradeInputError = '';
      }
    },
    checkCreditInput() {
      const credit = parseInt(this.newCourseCredit);
      if (isNaN(credit) || credit < 0 || credit > 30) {
        this.creditInputError = 'Invalid credit value. Credit must be between 1 and 30.';
      } else {
        this.creditInputError = '';
      }
    },
    calculateGPA() {
      let totalGradePoints = 0;
      let totalCredits = 0;
      for (let course of this.courses) {
        const credit = course.credit;
        const grade = course.grade;
        let gradePoint = 0;
        switch (grade) {
          case 'A': gradePoint = 4; break;
          case 'A-': gradePoint = 3.7; break;
          case 'B+': gradePoint = 3.3; break;
          case 'B': gradePoint = 3; break;
          case 'B-': gradePoint = 2.7; break;
          case 'C+': gradePoint = 2.3; break;
          case 'C': gradePoint = 2; break;
          case 'C-': gradePoint = 1.7; break;
          case 'D+': gradePoint = 1.3; break;
          case 'D': gradePoint = 1; break;
          case 'D-': gradePoint = 0.7; break;
          case 'F': gradePoint = 0; break;
        }
        totalGradePoints += credit * gradePoint;
        totalCredits += credit;
      }
      if (totalCredits === 0) {
        return NaN;
      }
    return totalGradePoints / totalCredits;
    }
  },
  computed: {
    selectedCourses() {
      return this.courses.filter(course => course.selected);
    },
    gpa() {
      return this.calculateGPA().toFixed(2); 
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.input-field {
  width: 200px; 
  padding: 8px; 
  margin: 5px; 
  border-radius: 5px; 
  text-align: center; 
}
.list {
  list-style-type: none;
  background: #131415;
  color: #a1b2c3;
  margin: 20px;
  padding: 10px 20px;
  border-radius: 15px;
  align-items: center;
  display: inline-block;
  cursor: pointer;
}
.list.selected {
  background: #f39e28;
  color:black;
  border-bottom: 2px solid red; 
}
.button {
  border-radius: 15px;
  background-color: #f39e28;
  color: black;
  padding: 10px 20px;
  margin: 20px 20px;
}
.material-symbols-outlined {
  font-variation-settings:
  'FILL' 0,
  'wght' 400,
  'GRAD' 0,
  'opsz' 24
}
</style>