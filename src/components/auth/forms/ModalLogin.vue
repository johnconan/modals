<template>
  <modal
    title="Login Form"
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
          <p class="errorText" v-if="!$v.password.required">password is required!</p>
          <p class="errorText" v-if="!$v.password.minLength"> Password must have at least {{ $v.password.$params.minLength.min }}</p>
          <input
            type="password"
            v-model="password"
            :class="{ error: $v.password.$error }"
            @change="$v.password.$touch()">
        </div>
        <button class="btn btnPrimary">Submit!</button>
      </form>
      <hr/>
      <a @click.prevent="$emit('selectModal')" href="#">Нужен аккаунт?</a>
    </div>
  </modal>
</template>
<script>
import modal from '@/components/UI/Modal.vue';
import { required, minLength, email } from 'vuelidate/lib/validators';

export default {
  components: { modal },
   data() {
    return {
      email: '',
      password: ''
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
        this.$v.$reset();
        this.$emit('close');
      }
    },
    clearForm() {
      this.$nextTick(() => {
        this.email = '';
        this.password = '';
        this.$v.$reset();
        this.$emit('close');
      })
    }
  }
}
</script>

<style lang="scss">
  .form-item .errorText {
    display: none;
    margin-bottom: 8px;
    font-size: 13.4px;
    color: red;
  }

  .form-item {
    &.errorInput .errorText {
      display: block;
    }
  }
  input.error {
    border: 1px solid red;
  } 

</style>