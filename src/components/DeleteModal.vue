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
      />
    </template>

    <v-card>
      <v-card-title class="text-h5">
        Вы уверены, что хотите удалить студента?
      </v-card-title>

      <v-card-actions>
        <v-spacer />
        <v-btn
          @click="showModal = false"
        >
          Отмена
        </v-btn>

        <v-btn
          color="primary"
          @click="deleteConfirm"
        >
          Удалить
        </v-btn>

        <v-spacer></v-spacer>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  data () {
    return {
      showModal: false
    }
  },

  methods: {
    deleteConfirm () {
      this.$emit('confirm')
    }
  }
}
</script>
