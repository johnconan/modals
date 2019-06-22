<template>
  <div class="wrapper">
    <div class="wrapper-content">

      <section>
        <div class="container">

          <!-- modal with validate -->
          <button 
            class="btn btnPrimary" 
            @click="modalValidate = !modalValidate">
            Show modal - validate
          </button>
          <modalValidate 
            v-show="modalValidate"
            @close="modalValidate = false"
          />

          <!-- modal with registration -->
          <button 
            class="btn btnPrimary" 
            @click="authModal.reg = !authModal.reg">
            Registration
          </button>
          <modalReg
            @close="authModal.reg = false"
            @selectModal="selectModal"
            v-show="authModal.reg"
          />

          <!-- modal with login -->
          <button 
            class="btn btnPrimary" 
            @click="authModal.login = !authModal.login">
            Login
          </button>
          <modalLogin
            @selectModal="selectModal"
            @close="authModal.login = false"
            v-show="authModal.login"
          />

        </div>
      </section>

    </div>
  </div>
</template>

<script>
import modalValidate from '@/components/auth/forms/ModalValidate.vue'
import modalReg from '@/components/auth/forms/ModalReg.vue'
import modalLogin from '@/components/auth/forms/ModalLogin.vue'

export default {
  components: { modalValidate, modalReg, modalLogin },
  data () {
    return {
      modalValidate: false,
      authModal: {
        login: false,
        reg: false
      }
    }
  },
  methods: {
    selectModal() {
      this.authModal.login = !this.authModal.login,
      this.authModal.reg = !this.authModal.reg
    }
  }
}
</script>
