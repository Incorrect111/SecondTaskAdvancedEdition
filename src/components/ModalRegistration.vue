<template>
  <modal title="Registration" @close="$emit('close')">
    <div slot="body">
      <form @submit.prevent="onSubmit">

        <!--Email -->

        <div class="form-item" :class="{ errorInput : $v.password.$error }">
          <label>Email:</label>
          <p class="errorText" v-if="!$v.email.required">Field is required!!!</p>
          <p class="errorText" v-if="!$v.email.email">Email is not correct !</p>
          <input v-model="email" :class="{ error : $v.email.$error }" @change="$v.email.$touch()" />
        </div>

        <!--  Password -->

        <div class="form-item" :class="{ errorInput : $v.password.$error }">
          <label>Password:</label>
          <p class="errorText" v-if="!$v.password.required">Field is required!!!</p>
          <p
            class="errorText"
            v-if="!$v.password.minLength"
          >Password must have at least {{ $v.password.$params.minLength.min }} letters.</p>
          <input
            v-model="password"
            :class="{ error : $v.password.$error }"
            @change="$v.password.$touch()"
          />
        </div>

        <!-- Repeat Password -->

        <div class="form-item" :class="{ errorInput : $v.repeatPassword.$error }">
          <label>Repeat password:</label>
          <p class="errorText" v-if="!$v.repeatPassword.required">Field is required!!!</p>
          <p class="errorText" v-if="!$v.repeatPassword.sameAsPassword">Passwords must be identical.</p>
          <input
            v-model="repeatPassword"
            :class="{ error : $v.repeatPassword.$error }"
            @change="$v.repeatPassword.$touch()"
          />
        </div>

        <!--Submit Button -->

        <button class="btn btnPrimary">Submit</button>
      </form>

      <!-- i have accaount button -->
      <button class="btn btnPrimary" @click="$emit('open-login')">I already have an account</button>
    </div>
  </modal>
</template>

<script>
import modal from "@/components/UI/Modal.vue";
import { required, minLength, email, sameAs } from "vuelidate/lib/validators";

export default {
  components: {
    modal
  },

  data() {
    return {
      email: "",
      password: "",
      repeatPassword: ""
    };
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
      required,
      sameAsPassword: sameAs("password")
    }
  },
  methods: {
    onSubmit() {
      this.$v.$touch();
      if (!this.$v.$invalid) {
        const user = {
          email: this.email,
          password: this.password,
          repeatPassword: this.repeatPassword
        };
        console.log(user);

        // Reset
        (this.email = ""),
          (this.password = ""),
          (this.repeatPassword = ""),
          this.$v.$reset(),
          // Close
          this.$emit("close");
      }
    },
    resetModalRegistration() {
      (this.email = ""),
        (this.password = ""),
        (this.repeatPassword = ""),
        this.$v.$reset();
    }
  }
};
</script>


<style lang="scss">
.form-item .errorText {
  display: none;
  margin-bottom: 8px;
  font-size: 13.4;
  color: #e73a3a;
}

.form-item {
  &.errorInput .errorText {
    display: block;
  }
}
input.error {
  border-color: #e73a3a;
}

</style>