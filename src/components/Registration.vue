<template>
  <modal title="Registration Form" @close="resetCloseForm">
    <!-- Body -->
    <div slot="body">
      <form @submit.prevent="onSubmit">
        <!-- Email -->
        <div class="form-item" :class="{ errorInput: $v.email.$error }">
          <label>e-mail:</label>
          <p class="errorText" v-if="!$v.email.required">Field is required!</p>
          <p class="errorText" v-if="!$v.email.email">email isn't correct!</p>
          <input v-model="email" :class="{error: $v.email.$error}" @change="$v.email.$touch()" />
        </div>

        <!-- password -->
        <div class="form-item" :class="{ errorInput: $v.pass.$error }">
          <label>Input your password:</label>
          <p class="errorText" v-if="!$v.pass.required">Field is required!</p>
          <p
            class="errorText"
            v-if="!$v.pass.minLength"
          >Length must have min {{ $v.pass.$params.minLength.min}} characters!</p>
          <input v-model="pass" :class="{error: $v.pass.$error}" @change="$v.pass.$touch()" />
        </div>

        <!-- password2 -->
        <div class="form-item" :class="{ errorInput: $v.pass2.$error }">
          <label>Repeat your password:</label>
          <p class="errorText" v-if="!$v.pass.sameAs">Passwords must be the same!</p>
          <input v-model="pass2" :class="{error: $v.pass2.$error}" @change="$v.pass2.$touch()" />
        </div>

        <!-- Button -->
        <button class="btn btnPrimary">Submit</button>

        <!-- Link to LogIn Form -->
        <hr />
        <a class="link-change" @click="goLogIn">I have an account already >> login</a>

      </form>
    </div>
  </modal>
</template>

<script>
import { required, minLength, email, sameAs } from "vuelidate/lib/validators";
import modal from "@/components/UI/Modal.vue";
export default {
  components: { modal },
  data() {
    return {
      email: "",
      pass: "",
      pass2: "",
    };
  },
  validations: {
    email: {
      required,
      email,
    },
    pass: {
      required,
      minLength: minLength(4),
    },
    pass2: {
      sameAsPassword: sameAs("pass"),
    },
  },
  methods: {
    resetCloseForm() {
      this.email = "";
      this.pass = "";
      this.pass2 = "";
      this.$v.$reset();
      this.$emit("close");
    },
    onSubmit() {
      this.$v.$touch();
      if (!this.$v.$invalid) {
        const user = {
          email: this.email,
          pass: this.pass,
        };
        console.log(user);
        this.resetCloseForm();
      }
    },
    goLogIn () {
        this.$emit('change')
    },
  },
};
</script>
