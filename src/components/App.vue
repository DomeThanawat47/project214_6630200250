<template>
  <div class="app-container">
    <nav class="navbar">
      <a href="#" @click.prevent="currentView = 'student'" :class="{ active: currentView === 'student' }">ข้อมูลนิสิต</a>
      <a href="#" @click.prevent="currentView = 'courses'" :class="{ active: currentView === 'courses' }">รายละเอียดการเรียน</a>
    </nav>
    
    <div class="content">
      <StudentInfo v-if="currentView === 'student'" />
      <CourseInfo v-if="currentView === 'courses'" />
    </div>
  </div>
</template>

<script>
import StudentInfo from './StudentInfo.vue'
import CourseInfo from './CourseInfo.vue'

export default {
  components: {
    StudentInfo,
    CourseInfo
  },
  data() {
    return {
      currentView: 'student'
    }
  }
}
</script>

<style scoped>
@import './styles/app.css';
</style>