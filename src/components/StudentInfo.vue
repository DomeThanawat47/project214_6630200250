<template>
    <div class="student-card">
      <h2>Student information</h2>
      <StudentImage v-if="student.images && student.images.length" :images="student.images" @add-image="addImage" />
      <div class="info">
        <p><span>Name:</span> {{ student.name || 'กำลังโหลด...' }}</p>
        <p><span>Student ID:</span> {{ student.studentId || 'กำลังโหลด...' }}</p>
        <p><span>Faculty:</span> {{ student.major || 'กำลังโหลด...' }}</p>
        <p><span>School:</span> {{ student.school || 'กำลังโหลด...' }}</p>
      </div>
      <button class="action-btn" @click="showEditForm = true">แก้ไข</button>
      
      <div v-if="showEditForm" class="edit-form">
        <form @submit.prevent="updateStudent">
          <input v-model="editForm.name" placeholder="ชื่อ-นามสกุล" required>
          <input v-model="editForm.studentId" placeholder="รหัสนิสิต" required>
          <input v-model="editForm.major" placeholder="สาขา" required>
          <input v-model="editForm.school" placeholder="โรงเรียนเดิม" required>
          <div class="form-buttons">
            <button type="submit" class="action-btn save-btn">บันทึก</button>
            <button type="button" class="action-btn cancel-btn" @click="showEditForm = false">ยกเลิก</button>
          </div>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios'
  import StudentImage from './StudentImage.vue'
  
  export default {
    components: {
      StudentImage
    },
    data() {
      return {
        student: {
          name: 'ธนวัฒน์ แสนบุตร',
          studentId: '6630200250',
          major: 'วิทยาการคอมพิวเตอร์',
          school: 'เทพศิรินทร์',
          images: [
            '/images/student1.jpg',
            '/images/student2.jpg',
            '/images/student3.jpg'
          ]
        },
        showEditForm: false,
        editForm: {
          name: '',
          studentId: '',
          major: '',
          school: ''
        }
      }
    },
    created() {
      this.fetchStudent()
    },
    methods: {
      async fetchStudent() {
        try {
          const response = await axios.get('http://localhost:3000/student')
          this.student = response.data
          this.editForm = { ...this.student }
        } catch (error) {
          console.error('Error fetching student data:', error)
        }
      },
      async updateStudent() {
        try {
          await axios.put('http://localhost:3000/student', {
            ...this.student,
            ...this.editForm
          })
          this.showEditForm = false
          this.fetchStudent()
        } catch (error) {
          console.error('Error updating student data:', error)
        }
      },
      addImage(imageUrl) {
        this.student.images.push(imageUrl)
      }
    }
  }
  </script>
  
  <style scoped>
  @import './styles/student-info.css';
  </style>