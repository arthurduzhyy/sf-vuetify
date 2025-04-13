<template>
  <v-container>
    <v-form ref="form" v-model="valid">
      <v-row>
        <v-col cols="12" sm="4">
          <v-text-field
              v-model="newStudent.name"
              label="ПІП"
              required
          />
        </v-col>
        <v-col cols="12" sm="4">
          <v-text-field
              v-model="newStudent.birthYear"
              label="Рік народження"
              type="number"
              required
          />
        </v-col>
        <v-col cols="12" sm="4">
          <v-text-field
              v-model="newStudent.phone"
              label="Телефон"
              required
          />
        </v-col>
        <v-col cols="12" sm="4">
          <v-text-field
              v-model="newStudent.email"
              label="Email"
              required
          />
        </v-col>
        <v-col cols="12">
          <v-btn
              :disabled="!valid"
              color="primary"
              @click="addStudent"
          >
            {{ editingIndex !== null ? 'Оновити студента' : 'Додати студента' }}
          </v-btn>

          <v-btn
              v-if="editingIndex !== null"
              color="secondary"
              @click="resetForm"
          >
            Скасувати редагування
          </v-btn>
        </v-col>

      </v-row>
    </v-form>

    <v-data-table
        :headers="headers"
        :items="students"
        item-value="name"
        v-model:items-per-page="itemsPerPage"
        :search="search"
        class="elevation-1"
    >
      <template v-slot:top>
        <v-text-field
            v-model="search"
            label="Пошук"
            append-icon="mdi-magnify"
            single-line
            hide-details
        />
      </template>

      <template v-slot:item.actions="{ item }">
        <v-btn icon @click="editStudent(item)">
          <v-icon>mdi-pencil</v-icon>
        </v-btn>
        <v-btn icon @click="deleteStudent(item)">
          <v-icon>mdi-delete</v-icon>
        </v-btn>
      </template>
    </v-data-table>
  </v-container>
</template>

<script setup lang="ts">
import { ref } from 'vue'

type Student = {
  name: string;
  birthYear: number;
  phone: string;
  email: string;
}

const newStudent = ref<Student>({
  name: '',
  birthYear: 0,
  phone: '',
  email: ''
})

const students = ref<Student[]>([
  { name: 'Іванов Іван Іванович', birthYear: 2000, phone: '123-456-7890', email: 'ivanov@example.com' },
  { name: 'Петров Петро Петрович', birthYear: 2001, phone: '987-654-3210', email: 'petrov@example.com' },
])

const headers = ref([
  { text: 'ПІП', align: 'start', value: 'name', sortable: true },
  { text: 'Рік народження', align: 'start', value: 'birthYear', sortable: true },
  { text: 'Телефон', align: 'start', value: 'phone', sortable: true },
  { text: 'Email', align: 'start', value: 'email', sortable: true },
  { text: 'Дії', align: 'center', value: 'actions' }
])

const search = ref('')
const itemsPerPage = ref(5)
const valid = ref(false)

const editingIndex = ref<number | null>(null)

function addStudent() {
  if (editingIndex.value !== null) {
    students.value[editingIndex.value] = { ...newStudent.value }
  } else {
    students.value.push({ ...newStudent.value })
  }

  resetForm()
}

function resetForm() {
  newStudent.value = {
    name: '',
    birthYear: 0,
    phone: '',
    email: ''
  }
  valid.value = false
  editingIndex.value = null
}

function editStudent(item: Student) {
  const index = students.value.indexOf(item)
  if (index !== -1) {
    newStudent.value = { ...item }
    editingIndex.value = index
    valid.value = true
  }
}

function deleteStudent(item: Student) {
  const index = students.value.indexOf(item)
  if (index !== -1) {
    students.value.splice(index, 1)
    if (editingIndex.value === index) {
      resetForm()
    }
  }
}
</script>

<style scoped>
.v-btn {
  margin-left: 5px;
}
</style>
