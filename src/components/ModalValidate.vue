<template>
  <modal
  title="Form modal"
    @close="resetCloseForm">
    <!-- Body -->
    <div slot="body">
      <form @submit.prevent="onSubmit">
        
        <!-- Name -->
        <div class="form-item" :class="{ errorInput: $v.name.$error }">
          <label>Name:</label>
          <p class="errorText" v-if="!$v.name.required">Field is required!</p>
          <p class="errorText" v-if="!$v.name.minLength">Length must have min {{ $v.name.$params.minLength.min}} letters! </p>
          <input
          v-model="name"
          :class="{error: $v.name.$error}"
          @change="$v.name.$touch()"
          />
        </div>
        
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

        <!-- password2 -->
        <div class="form-item" :class="{ errorInput: $v.pass2.$error }">
          <label>Repeat your password:</label>
          <p class="errorText" v-if="!$v.pass.sameAs">Passwords must be the same!</p>
          <input
          v-model="pass2"
          :class="{error: $v.pass2.$error}"
          @change="$v.pass2.$touch()"
          />
        </div>

        <!-- Button -->
        <button class="btn btnPrimary">Submit</button>
      </form>
    </div>
  </modal>
</template>

<script>
import { required, minLength, email, sameAs } from 'vuelidate/lib/validators'
import modal from "@/components/UI/Modal.vue";
export default {
  components: { modal },
  data () {
    return {
      email: '',
      name: '',
      pass: '',
      pass2: '',
    }
  },
  validations: {
    name: {
      required,
      minLength: minLength(4)
    },
    email: {
      required,
      email,
    },
    pass: {
      required,
      minLength: minLength(4)
    },
    pass2: {
      sameAsPassword: sameAs('pass')
    }
  },
  methods: {
    resetCloseForm() {
      this.name = ''
      this.email = ''
      this.pass = ''
      this.pass2 = ''
      this.$v.$reset()
      this.$emit('close')
    },
    onSubmit() {
      this.$v.$touch()
      if (!this.$v.$invalid) {
        const user = {
          name: this.name,
          email: this.email,
          pass: this.pass,
        }
        console.log(user)
        this.resetCloseForm()
      }
    },
  },
}
</script>

<style lang="scss">
.form-item .errorText {
  display: none;
  margin-bottom: 8px;
  font-size: 13.4px;
  color: #de4040;
}

.form-item {
  &.errorInput .errorText {
  display: block;
  }
}

input.error {
  border-color: #de4040;
}
</style>
