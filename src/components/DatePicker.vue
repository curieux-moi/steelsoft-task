<template>
  <v-menu
    ref="menu"
    v-model="showDatePickerMenu"
    transition="scale-transition"
    :close-on-content-click="false"
    offset-y
    min-width="auto"
  >
    <template v-slot:activator="{ on, attrs }">
      <v-text-field
        :value="formattedDate"
        label="Выберите дату рождения"
        append-icon="mdi-calendar"
        readonly
        v-bind="attrs"
        v-on="on"
      />
    </template>
    <v-date-picker
      no-title
      scrollable
      v-model="internalValue"
    >
      <v-spacer></v-spacer>
      <v-btn
        text
        @click="onClose"
      >
        Отменить
      </v-btn>

      <v-btn
        text
        color="primary"
        @click="onSave"
      >
        Применить
      </v-btn>
    </v-date-picker>
  </v-menu>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      default: ''
    }
  },

  data () {
    return {
      internalValue: this.value,
      showDatePickerMenu: false
    }
  },

  computed: {
    formattedDate () {
      if (!this.internalValue) return
      const date = new Date(this.internalValue)
      return new Intl.DateTimeFormat('ru-RU').format(date)
    }
  },

  methods: {
    onSave () {
      this.$emit('input', this.internalValue)
      this.showDatePickerMenu = false
    },

    onClose () {
      this.showDatePickerMenu = false
      this.internalValue = this.value
    }
  }
}
</script>
