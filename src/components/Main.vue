<template>
  <div class="main">
    <v-data-table
      :headers="columns"
      :items="filteredStudents"
      :search="filters.search"
    >
      <template #top>
        <v-toolbar
          flat
        >
          <filters
            v-model="filters"
            @clear="onFiltersReset"
          />
        </v-toolbar>
      </template>

      <template #item.birthDate="{ item: { birthDate } }">
        {{ formatDate(birthDate) }}
      </template>

      <template #item.sections="{ item: { sections } }">
        <div
          v-for="section in sections"
          :key="section.id"
        >
          {{ section.name }}
          Вступил {{ formatDate(section.enteredAt) }}
        </div>
      </template>

      <template #item.enrolled="{ item: { enrolled } }">
        <v-chip
          outlined
          :color="enrolled ? 'green' : 'red'"
        >
          {{ enrolled ? 'Зачислен' : 'Отчислен' }}
        </v-chip>
      </template>

      <template #item.actions="{ item }">
        <add-modal
          :student="item"
          @submit="onStudentChange"
        >
          <template #activator="{ on, attrs }">
            <v-icon
              small
              class="mr-2"
              v-on="on"
              v-bind="attrs"
            >
              mdi-pencil
            </v-icon>
          </template>
        </add-modal>

        <delete-modal @confirm="deleteStudent(item)">
          <template #activator="{ on, attrs }">
            <v-icon
              small
              v-on="on"
              v-bind="attrs"
            >
              mdi-delete
            </v-icon>
          </template>
        </delete-modal>
      </template>

      <template #footer>
        <v-toolbar
          rounded
        >
          <v-row justify="center">
            <add-modal
              @submit="onAddStudent"
            />
          </v-row>
        </v-toolbar>
      </template>
    </v-data-table>
  </div>
</template>

<script>
import students from '@/mocks/students.json'
import columns from '@/constants/columns'
import AddModal from '@/components/AddModal.vue'
import DeleteModal from '@/components/DeleteModal.vue'
import Filters from '@/components/Filters.vue'

export default {
  components: {
    AddModal,
    DeleteModal,
    Filters
  },

  data: () => ({
    columns,
    students,
    filters: {
      search: null,
      enrolled: null,
      birthDate: null,
      educationForm: null
    }
  }),

  computed: {
    filteredStudents () {
      const activeFilters = Object.entries(this.filters)
        .reduce((res, [key, value]) => {
          if (value && key !== 'search') {
            res[key] = value
          }
          return res
        }, {})
      const filterKeys = Object.keys(activeFilters)

      return this.students.filter(student => {
        if (filterKeys.length) {
          return filterKeys.every(key => student[key] === activeFilters[key])
        }
        return student
      })
    }
  },

  methods: {
    formatDate (date) {
      if (!date) return ''
      return new Intl.DateTimeFormat('ru-RU').format(new Date(date))
    },

    findStudentIdx (value) {
      return this.students.findIndex(student => student.id === value.id)
    },

    onAddStudent (value) {
      this.students.unshift(value)
    },

    onStudentChange (value) {
      const idx = this.findStudentIdx(value)
      this.$set(this.students, idx, value)
    },

    deleteStudent (value) {
      const idx = this.findStudentIdx(value)
      this.students.splice(idx, 1)
    },

    onFiltersReset () {
      Object.keys(this.filters).forEach(key => (this.filters[key] = null))
    }
  }
}
</script>
