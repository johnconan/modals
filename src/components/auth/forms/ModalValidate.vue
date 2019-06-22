<template>
  <modal
    title="Modal with form + Validate"
    @close="clearForm">
    <!-- body -->
    <div slot="body">
      <form @submit.prevent="onSubmit">

        <!-- name -->
        <div class="form-item" :class="{ errorInput: $v.name.$error }">
          <label>Name:</label>
          <p class="errorText" v-if="!$v.name.required"> Filed is required! </p>
          <p class="errorText" v-if="!$v.name.minLength"> Name must have at least {{ $v.name.$params.minLength.min }} !</p>
          <input
            v-model="name"
            :class="{ error: $v.name.$error }"
            @change="$v.name.$touch()">
        </div>

        <!-- email -->
        <div class="form-item" :class="{ errorInput: $v.email.$error }">
          <label>Email:</label>
          <p class="errorText" v-if="!$v.email.required"> Filed is required! </p>
          <p class="errorText" v-if="!$v.email.email"> Email is not correct!</p>
          <input
            v-model="email"
            :class="{ error: $v.email.$error }"
            @change="$v.email.$touch()">
        </div>

        <div class="form-item" :class="{ errorInput: $v.password.$error }">
          <label>Password</label>
          <p class="errorText" v-if="!$v.password.required">Password is required!</p>
          <p class="errorText" v-if="!$v.password.minLength"> Password must have at least {{ $v.password.$params.minLength.min }}</p>
          <input
            type="password"
            v-model="password"
            :class="{ error: $v.password.$error }"
            @change="$v.password.$touch()">
        </div>

        <div class="form-item" :class="{ errorInput: $v.repeatPassword.$error }">
          <label>Repeat Password</label>
          <p class="errorText" v-if="!$v.repeatPassword.sameAsPassword"> Passwords must be identical</p>
          <input
            type="password"
            v-model="repeatPassword"
            :class="{ error: $v.repeatPassword.$error }"
            @change="$v.repeatPassword.$touch()">
        </div>
        <button class="btn btnPrimary">Submit!</button>
      </form>
    </div>
  </modal>
</template>

<script>
import { required, minLength, email, sameAs } from 'vuelidate/lib/validators'

import modal from '@/components/UI/Modal.vue'

export default {
  components: { modal },
  data () {
    return {
      name: '',
      email: '',
      password: '',
      repeatPassword: ''
    }
  },
  validations: {
    name: {
      required,
      minLength: minLength(4)
    },
    email: {
      required,
      email
    },
    password: {
      required,
      minLength: minLength(6)
    },
    repeatPassword: {
      sameAsPassword: sameAs('password')
    }
  },
  methods: {
    onSubmit () {
      this.$v.$touch()
      if (!this.$v.$invalid) {
        const user = {
          name: this.name,
          email: this.email,
          password: this.password
        }
        console.log(user)

        // DONE!
        this.name = '';
        this.email = '';
        this.password = '';
        this.repeatPassword = '';
        this.$v.$reset()
        this.$emit('close')
      }
    },
    clearForm() {
      this.$emit('close');
      this.name = '';
      this.email = '';
      this.password = '';
      this.repeatPassword = '';
      this.$nextTick(() => {
        this.$v.$reset()
      })
    }
  }
}
</script>


