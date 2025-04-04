<template>
  <div class="course-card">
    <h2>รายละเอียดการเรียน</h2>
    <button class="action-btn add-btn" @click="showAddForm = true">เพิ่ม</button>
    <table>
      <thead>
        <tr>
          <th>รหัสวิชา</th>
          <th>ชื่อวิชา</th>
          <th>หน่วยกิต</th>
          <th>เกรด</th>
          <th>การกระทำ</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="course in courses" :key="course.id">
          <td>{{ course.code }}</td>
          <td>{{ course.name }}</td>
          <td>{{ course.credits }}</td>
          <td>{{ course.grade }}</td>
          <td>
            <button class="action-btn edit-btn" @click="editCourse(course)">แก้ไข</button>
            <button class="action-btn delete-btn" @click="deleteCourse(course.id)">ลบ</button>
          </td>
        </tr>
      </tbody>
    </table>
    <div v-if="showAddForm || showEditForm" class="course-form">
      <form @submit.prevent="showAddForm ? addCourse() : updateCourse()">
        <input v-model="form.code" placeholder="รหัสวิชา" required>
        <input v-model="form.name" placeholder="ชื่อวิชา" required>
        <input v-model="form.credits" placeholder="หน่วยกิต" type="number" required>
        <input v-model="form.grade" placeholder="เกรด" required>
        <div class="form-buttons">
          <button type="submit" class="action-btn save-btn">{{ showAddForm ? 'เพิ่ม' : 'บันทึก' }}</button>
          <button type="button" class="action-btn cancel-btn" @click="cancelForm">ยกเลิก</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      courses: [],
      showAddForm: false,
      showEditForm: false,
      form: {
        id: null,
        code: '',
        name: '',
        credits: '',
        grade: ''
      }
    }
  },
  created() {
    this.fetchCourses()
  },
  methods: {
    async fetchCourses() {
      try {
        const response = await axios.get('http://localhost:3000/courses')
        this.courses = response.data
      } catch (error) {
        console.error('Error fetching courses:', error)
      }
    },
    async addCourse() {
      try {
        const newCourse = {
          code: this.form.code,
          name: this.form.name,
          credits: Number(this.form.credits),
          grade: this.form.grade
        }
        await axios.post('http://localhost:3000/courses', newCourse)
        this.showAddForm = false
        this.resetForm()
        this.fetchCourses()
      } catch (error) {
        console.error('Error adding course:', error)
      }
    },
    async updateCourse() {
      try {
        const updatedCourse = {
          id: this.form.id,
          code: this.form.code,
          name: this.form.name,
          credits: Number(this.form.credits),
          grade: this.form.grade
        }
        await axios.put(`http://localhost:3000/courses/${this.form.id}`, updatedCourse)
        this.showEditForm = false
        this.resetForm()
        this.fetchCourses()
      } catch (error) {
        console.error('Error updating course:', error)
      }
    },
    async deleteCourse(id) {
      try {
        await axios.delete(`http://localhost:3000/courses/${id}`)
        this.fetchCourses()
      } catch (error) {
        console.error('Error deleting course:', error)
      }
    },
    editCourse(course) {
      this.form = { ...course }
      this.showEditForm = true
      this.showAddForm = false
    },
    resetForm() {
      this.form = { id: null, code: '', name: '', credits: '', grade: '' }
    },
    cancelForm() {
      this.showAddForm = false
      this.showEditForm = false
      this.resetForm()
    }
  }
}
</script>

<style scoped>
@import './styles/course-info.css';
</style>