<script setup lang="ts">
import type { Student } from '@/types'
import { ref, onMounted } from 'vue'
import StudentService from '@/services/StudentService'
import StudentCard from '@/components/StudentCard.vue'

const students = ref<Student[]>([])

onMounted(() => {
  StudentService.getStudents()
    .then((response) => {
      console.log('Students:', response.data)
      students.value = response.data
    })
    .catch((error) => {
      console.error('Failed to load students:', error)
    })
})
</script>

<template>
  <h1>Student List</h1>
  <div class="student-cards">
    <StudentCard v-for="student in students" :key="student.id" :student="student" />
  </div>
</template>

<style scoped>
.student-cards {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
