<template>
  <form @submit.prevent="submitForm">
    <div class="form-row">
        <input type="text" class="form-control col-3 mx-2" placeholder="Name"
        v-model='student.name'>
        <input type="text" class="form-control col-3 mx-2" placeholder="Course"
        v-model='student.course'>
        <input type="text" class="form-control col-3 mx-2" placeholder="Rating"
        v-model='student.rating'>
        <button class="btn btn-success">Submit</button>
    </div>
  </form>

  <table class="table mt-5">
    <thead>
      <tr>
        <th scope="col">Name</th>
        <th scope="col">Course</th>
        <th scope="col">Rating</th>
        <th scope="col"></th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="student in students" :key="student.id" @dblclick="$data.student=student">
        <td>{{ student.name }}</td>
        <td>{{ student.course }}</td>
        <td>{{ student.rating }}</td>
        <td>
          <button class="btn btn-outline-danger btn-small mx-1" @click="deleteStudent(student)">x</button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      student: {},
      students: [],
    }
  },

  async created() {
    await this.getStudents();
  },

  methods: {
    submitForm() {
      if (this.student.id === undefined) {
        this.createStudent();
      } else {
        this.updateStudent();
      }
    },

    async getStudents() {
      let resp = await fetch("http://127.0.0.1:8000/api/students/");
      this.students = await resp.json();
    },

    async createStudent() {
      await this.getStudents();

      await fetch("http://127.0.0.1:8000/api/students/", {
        method: "POST",
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.student)
      })

      await this.getStudents();
      this.student = {};
    },

    async updateStudent() {
      await this.getStudents();

      await fetch(`http://127.0.0.1:8000/api/students/${this.student.id}/`, {
        method: "PUT",
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.student)
      })

      await this.getStudents();
      this.student = {};
    },

    async deleteStudent(student) {
      await this.getStudents();

      await fetch(`http://127.0.0.1:8000/api/students/${student.id}/`, {
        method: "DELETE",
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.student)
      })

      await this.getStudents();
    }
  }, 

}
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
</style>
