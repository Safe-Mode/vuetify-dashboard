<template>
  <v-container>
    <v-row>
      <v-col>
        <h1>Signup</h1>

        <v-form ref="signUpForm" v-model="formValidity">
          <v-text-field
            type="email"
            label="Email"
            v-model="email"
            :rules="emailRules"
          ></v-text-field>

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

          <v-checkbox
            label="Agree to terms and conditions"
            v-model="agreeToTerms"
            :rules="agreeToTermsRules"
            required
          ></v-checkbox>

          <v-btn
            type="submit"
            color="primary"
            class="mr-4"
            :disabled="!formValidity"
            >Submit</v-btn
          >
          <v-btn
            type="button"
            color="success"
            class="mr-4"
            @click="validateForm"
            >Validate</v-btn
          >
          <v-btn
            type="button"
            color="warning"
            class="mr-4"
            @click="resetValidation"
            >Reset Validation</v-btn
          >
          <v-btn type="reset" color="error" @click="resetForm">Reset</v-btn>
        </v-form>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'Signup',
  data: () => ({
    email: '',
    emailRules: [
      value => value.indexOf('@') !== 0 || 'Email should have a username.',
      value => value.includes('@') || 'Email should include an @ symbol.',
      value =>
        value.indexOf('.') - value.indexOf('@') > 1 ||
        'Email should contain a valid domain.',
      value => value.includes('.') || 'Email should include a period symbol.',
      value =>
        value.indexOf('.') <= value.length - 3 ||
        'Email should contain a valid domain extension.'
    ],
    browsers: ['Chrome', 'Firefox', 'Safari', 'Edge', 'Brave'],
    date: null,
    menu: false,
    agreeToTerms: false,
    agreeToTermsRules: [
      value =>
        !!value ||
        'You must agree to the terms and conditions to sign up for an account.'
    ],
    formValidity: false
  }),
  watch: {
    menu(val) {
      val && setTimeout(() => (this.$refs.picker.activePicker = 'YEAR'))
    }
  },
  methods: {
    save(date) {
      this.$refs.menu.save(date)
    },
    resetForm() {
      this.$refs.signUpForm.reset()
    },
    resetValidation() {
      this.$refs.signUpForm.resetValidation()
    },
    validateForm() {
      this.$refs.signUpForm.validate()
    }
  }
}
</script>
