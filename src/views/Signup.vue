<template>
  <v-container>
    <v-row>
      <v-col>
        <h1>Signup</h1>

        <v-form>
          <v-text-field type="email" label="Email"></v-text-field>

          <v-autocomplete
            label="Which browser do you use?"
            :items="browsers"
          ></v-autocomplete>

          <v-file-input label="File input"></v-file-input>

          <v-menu
            ref="menu"
            v-model="menu"
            :close-on-content-click="false"
            transition="scale-transition"
            offset-y
            full-width
            min-width="290px"
          >
            <template v-slot:activator="{ on }">
              <v-text-field
                v-model="date"
                label="Birthday date"
                prepend-icon="event"
                readonly
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker
              ref="picker"
              v-model="date"
              :max="new Date().toISOString().substr(0, 10)"
              min="1950-01-01"
              @change="save"
            ></v-date-picker>
          </v-menu>

          <v-checkbox label="Agree to terms and conditions"></v-checkbox>

          <v-btn type="submit" color="primary">Submit</v-btn>
        </v-form>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'Signup',
  data: () => ({
    browsers: ['Chrome', 'Firefox', 'Safari', 'Edge', 'Brave'],
    date: null,
    menu: false
  }),
  watch: {
    menu(val) {
      val && setTimeout(() => (this.$refs.picker.activePicker = 'YEAR'))
    }
  },
  methods: {
    save(date) {
      this.$refs.menu.save(date)
    }
  }
}
</script>
