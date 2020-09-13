<template>
  <modal
  title="Login Form"
    @close="resetCloseForm">
    <!-- Body -->
    <div slot="body">
      <form @submit.prevent="onSubmit">
        
        <!-- Email -->
        <div class="form-item" :class="{ errorInput: $v.email.$error }">
          <label>e-mail:</label>
          <p class="errorText" v-if="!$v.email.required">Field is required!</p>
          <p class="errorText" v-if="!$v.email.email">email isn't correct! </p>
          <input
          v-model="email"
          :class="{error: $v.email.$error}"
          @change="$v.email.$touch()"
          />
        </div>

        <!-- password -->
        <div class="form-item" :class="{ errorInput: $v.pass.$error }">
          <label>Input your password:</label>
          <p class="errorText" v-if="!$v.pass.required">Field is required!</p>
          <p class="errorText" v-if="!$v.pass.minLength"> Length must have min {{ $v.pass.$params.minLength.min}} characters!</p>
          <input
          v-model="pass"
          :class="{error: $v.pass.$error}"
          @change="$v.pass.$touch()"
          />
        </div>

        <!-- Button -->
        <button class="btn btnPrimary">Submit</button>
      </form>

			<!-- Link to Registration Form -->
			<hr>
			<a class="link-change" @click="goRegistration">I need to create an account</a>

    </div>
  </modal>
</template>

<script>
import { required, minLength, email } from 'vuelidate/lib/validators'
import modal from "@/components/UI/Modal.vue";
export default {
  components: { modal },
  data () {
    return {
      email: '',
      pass: '',
    }
  },
  validations: {
    email: {
      required,
      email,
    },
    pass: {
      required,
      minLength: minLength(4)
    },
  },
  methods: {
    resetCloseForm() {
      this.email = ''
      this.pass = ''
      this.$v.$reset()
      this.$emit('close')
    },
    onSubmit() {
      this.$v.$touch()
      if (!this.$v.$invalid) {
        const user = {
          email: this.email,
          pass: this.pass,
        }
        console.log(user)
        this.resetCloseForm()
      }
		},
		goRegistration() {
			this.$emit('change')
		},
  },
}
</script>

<style lang="scss">
/* mouse over link */
.link-change:hover {
  font-weight: bold;
}

/* selected link */
.link-change {
	color: blue;
	text-decoration: underline;
	cursor: pointer;
}
</style>