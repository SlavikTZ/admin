<template lang="pug">
    v-layout(row='')
        v-flex(xs12='', sm8='', offset-sm2='', lg4='', offset-lg4='')
            v-card
                progress-bar(:show='form.busy')
                form(@submit.prevent='register', @keydown='form.onKeydown($event)')
                    v-card-title(primary-title='')
                        h3.headline.mb-0 {{ $t('register') }}
                    v-card-text
                        // Name
                        text-input(:form='form',
                        :label="$t('name')",
                        :v-errors='errors',
                        :value.sync='form.name',
                        browser-autocomplete='name',
                        counter='30', name='name',
                        v-validate="'required|max:30'")

                        // Email
                        email-input(:form='form',
                        :label="$t('email')",
                        :v-errors='errors',
                        :value.sync='form.email',
                        name='email',
                        v-validate="'required|email'")

                        // Password
                        password-input(:form='form',
                        :hint="$t('password_length_hint')",
                        :v-errors='errors',
                        :value.sync='form.password',
                        browser-autocomplete='new-password',
                        v-on:eye='eye = $event',
                        name='password',
                        v-validate="'required|min:8'")

                        // Password Confirmation
                        password-input(:form='form',
                        :hide='eye',
                        :label="$t('confirm_password')",
                        :v-errors='errors',
                        :value.sync='form.password_confirmation',
                        browser-autocomplete='new-password',
                        data-vv-as='password',
                        hide-icon='true',
                        name='password_confirmation',
                        v-validate="'required'")

                    v-card-actions
                        submit-button(:form='form', :label="$t('register')")
</template>

<script>
import Form from 'vform'

export default {
  name: 'register-view',
  metaInfo () {
    return { title: this.$t('register') }
  },

  data: () => ({
    form: new Form({
      name: '',
      email: '',
      password: '',
      password_confirmation: ''
    }),
    eye: true
  }),

  methods: {
    async register () {
      if (await this.formHasErrors()) return

      // Register the user.
      const { data } = await this.form.post('/api/register')

      // Log in the user.
      const { data: { token }} = await this.form.post('/api/login')

      // Save the token.
      this.$store.dispatch('saveToken', { token })

      // Update the user.
      await this.$store.dispatch('updateUser', { user: data })

      // Redirect home.
      this.$router.push({ name: 'home' })
    }
  }
}
</script>
