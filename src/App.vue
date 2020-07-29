<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">
          <button class="btn btnDefault" @click="modalLogin =! modalLogin">Login</button>
          <!-- <button class = btn btnWhite>Registration</button> -->

          <modalLogin
            v-show="modalLogin"
            @close="$refs.ModalLogin.resetModalLogin(),
          modalLogin = false"
            ref="ModalLogin"
            @open-registration="openRegistration"
          />
          <button
            class="btn btnDefault"
            @click="modalRegistration =! modalRegistration"
          >Registration</button>

          <modalRegistration
            v-show="modalRegistration"
            @close="$refs.ModalRegistration.resetModalRegistration(),
            modalRegistration = false"
            ref="ModalRegistration"
            @open-login="openLogin"
          />
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import modal from "@/components/UI/Modal.vue";
import modalLogin from "@/components/ModalLogin.vue";
import modalRegistration from "@/components/ModalRegistration.vue";

export default {
  components: {
    modal,
    modalLogin,
    modalRegistration
  },
  data() {
    return {
      modalLogin: false,
      modalRegistration: false
    };
  },
  methods: {
    openRegistration() {
      (this.modalLogin = false),
        (this.modalRegistration = true),
        this.$refs.ModalLogin.resetModalLogin();
    },
    openLogin() {
      (this.modalRegistration = false),
        (this.modalLogin = true),
        this.$refs.ModalRegistration.resetModalRegistration();
    }
  }
};
</script>

<style>
</style>
