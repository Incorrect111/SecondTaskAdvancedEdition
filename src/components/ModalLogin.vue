<template>
    <modal title="Login modal" @close="$emit('close')">
    <div slot="body">
      <!-- <p>Text TextText Text Text Text </p> -->
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

        <!-- Submit Button -->

        <button class="btn btnPrimary">Submit</button>
      </form>

      <!-- i need registration button -->

        <button class="btn btnPrimary" @click="$emit('open-registration')">I need registration</button>

    </div>
  </modal>
</template>

<script>
import modal from '@/components/UI/Modal.vue'
import { required, minLength, email, sameAs } from "vuelidate/lib/validators";

export default {

    components: {
        modal
    },

     data() {
    return {
      email: "",
      password: ""
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
    }

  },
    methods: {
    onSubmit() {
      this.$v.$touch();
      if (!this.$v.$invalid) {
        const user = {
          email: this.email,
          password: this.password
        };
 console.log(user);

        // Reset
          (this.email = ""),
          (this.password = "")
          this.$v.$reset(),
          // Close
          this.$emit("close");
      }

    },
    resetModalLogin() {
        (this.email = ""),
        (this.password = ""),
        this.$v.$reset()
    }
  }

}
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


// .link {
//     margin-right: 60px;
// }
</style>