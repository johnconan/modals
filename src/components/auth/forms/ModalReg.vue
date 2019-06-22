<template>
  <modal
    title="Registration Form"
    @close="clearForm"
  >
  <div slot="body">
      <form @submit.prevent="onSubmit">
      <div class="form-item" :class="{ errorInput: $v.email.$error }">
        <label>Email</label>
        <p class="errorText" v-if="!$v.email.required">Email is required!</p>
        <p class="errorText" v-if="!$v.email.email"> invalid format email!</p>
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
      <hr/>
      <a @click.prevent="$emit('selectModal')" href="">Есть аккаунт?</a>
    </form>
  </div>

  </modal>
</template>
<script>
import modal from '@/components/UI/Modal.vue';
import { required, minLength, email, sameAs } from 'vuelidate/lib/validators';

export default {
  components: { modal },
  data() {
    return {
      email: '',
      password: '',
      repeatPassword: ''
    }
  },
  validations: {
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
    onSubmit() {
      this.$v.$touch();
      if (!this.$v.$invalid) {
        const user = {
          email: this.email,
          password: this.password
        }
        console.log(user);
        this.email = '';
        this.password = '';
        this.repeatPassword = '';
        this.$v.$reset();
        this.$emit('close');
      }
    },
    clearForm() {
      this.$emit('close');
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