<template>
  <v-row>
    <v-col cols="2.5">
      <v-text-field
        :value="value.search"
        append-icon="mdi-magnify"
        label="Поиск"
        single-line
        hide-details
        @input="onInput('search', $event)"
      />
    </v-col>

    <v-col cols="2.5">
      <date-picker
        :value="value.birthDate"
        @input="onInput('birthDate', $event)"
      />
    </v-col>

    <v-col cols="2.5" class="mt-2">
      <v-checkbox
        label="Только зачисленные"
        :value="value.enrolled"
        @change="onInput('enrolled', $event)"
      />
    </v-col>

    <v-col cols="2.5" class="mt-n4">
      Форма обучения

      <v-radio-group
        :value="value.educationForm"
        @change="onInput('educationForm', $event)"
      >
        <v-row>
          <v-col cols="2.5">
            <v-radio
              value="Очная"
              label="Очная"
            />
          </v-col>

          <v-col cols="2.5">
            <v-radio
              value="Заочная"
              label="Заочная"
            />
          </v-col>
        </v-row>
      </v-radio-group>
    </v-col>

    <v-col cols="2.5">
      <v-btn
        @click="$emit('clear')"
      >
        Очистить фильтры
      </v-btn>
    </v-col>
  </v-row>
</template>

<script>
import DatePicker from '@/components/DatePicker.vue'

export default {
  components: { DatePicker },

  props: {
    value: {
      type: Object,
      default: null
    }
  },

  data: () => ({
    showDatePickerMenu: false
  }),

  methods: {
    onInput (fieldName, value) {
      this.$emit('input', {
        ...this.value,
        [fieldName]: value
      })
    }
  }
}
</script>
