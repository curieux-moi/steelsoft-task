<template>
  <v-dialog
    v-model="showModal"
    max-width="600px"
  >
    <template #activator="{ on, attrs }">
      <slot
        name="activator"
        :on="on"
        :attrs="attrs"
      >
        <v-btn
          color="primary"
          dark
          class="mb-2"
          v-bind="attrs"
          v-on="on"
        >
          Добавить студента
        </v-btn>
      </slot>
    </template>

    <v-card>
      <v-form
        ref="form"
        v-model="valid"
      >
        <v-card-title>
          <span class="text-h5">{{ formTitle }}</span>
        </v-card-title>

        <v-card-text>
          <v-container>
            <v-row>
              <v-col
                cols="6"
              >
                ФИО

                <v-text-field
                  v-model="newStudent.fullName"
                  label="Введите ФИО студента"
                  :rules="requiredRules"
                />
              </v-col>

              <v-col
                cols="6"
              >
                Дата рождения

                <date-picker
                  v-model="newStudent.birthDate"
                  :rules="requiredRules"
                />
              </v-col>

              <v-col
                cols="6"
              >
                Секции

                <v-combobox
                  v-model="sectionsModel"
                  :items="sectionsOptions"
                  label="Выберите секции"
                  multiple
                  chips
                  :rules="requiredRules"
                />
              </v-col>

              <v-col
                cols="6"
              >
                Статус

                <v-checkbox
                  label="Зачислен"
                  v-model="newStudent.enrolled"
                />
              </v-col>

              <v-col
                v-if="newStudent.enrolled"
                cols="6"
              >
                Форма обучения

                <v-radio-group v-model="newStudent.educationForm">
                  <v-radio
                    value="Очная"
                    label="Очная"
                  />

                  <v-radio
                    value="Заочная"
                    label="Заочная"
                  />
                </v-radio-group>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer />
          <v-btn
            color="blue darken-1"
            text
            @click="showModal = false"
          >
            Отменить
          </v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="onSubmit"
          >
            Сохранить
          </v-btn>
        </v-card-actions>
      </v-form>
    </v-card>
  </v-dialog>
</template>

<script>
import sections from '@/constants/sections'
import DatePicker from '@/components/DatePicker.vue'

const defaultStudentValue = {
  fullName: '',
  birthDate: '',
  sections: [],
  enrolled: false,
  educationForm: 'Очная'
}

export default {
  components: {
    DatePicker
  },

  props: {
    student: {
      type: Object,
      default: () => defaultStudentValue
    }
  },

  data () {
    return {
      valid: false,
      showModal: false,
      showDatePickerMenu: false,
      newStudent: { ...this.student },
      requiredRules: [
        v => !!v.length || 'Это поле обязательно'
      ]
    }
  },

  methods: {
    onSubmit () {
      const valid = this.$refs.form.validate()
      if (!valid) return
      this.$emit('submit', this.newStudent)
      this.showModal = false
    }
  },

  computed: {
    formTitle () {
      return this.student ? 'Изменить студента' : 'Добавить студента'
    },

    formattedBirthDate () {
      if (!this.newStudent.birthDate) return
      const date = new Date(this.newStudent.birthDate)
      return new Intl.DateTimeFormat('ru-RU').format(date)
    },

    sectionsModel: {
      get () {
        return this.newStudent.sections.map(section => section.name)
      },

      set (value) {
        this.newStudent.sections = value.map(name => {
          const existingSection = this.student.sections.find(s => s.name === name)

          if (existingSection) {
            return existingSection
          }

          const section = sections.find(s => s.name === name)

          return { ...section, enteredAt: new Date() }
        })
      }
    },

    sectionsOptions () {
      return sections.map(section => section.name)
    }
  }
}
</script>
