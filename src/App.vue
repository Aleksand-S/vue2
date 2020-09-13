<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">
          <!-- First modal -->
          <button class="btn btnPrimary" @click="modalFirst = !modalFirst">Show first modal</button>
          <modals
          title="First modal"
          v-show="modalFirst"
          @close="modalFirst = false">
            <div slot="body">
              <p>Test Test</p>
              <button class="btn btnPrimary" @click="modalFirst = !modalFirst">Done!</button>
            </div>
          </modals>

          <!-- Second modal -->
          <button
            class="btn btnPrimary"
            @click="modalSecond.show = !modalSecond.show"
          >Show form modal</button>
          <modals title="Form modal" v-show="modalSecond.show" @close="modalSecondClose">
            <div slot="body">
              <form @submit.prevent="submitSecondForm">
                <label>Name:</label>
                <input type="text" required v-model="modalSecond.name" />
                <label>e-mail:</label>
                <input type="email" required v-model="modalSecond.email" />
                <button class="btn btnPrimary">Submit</button>
              </form>
            </div>
          </modals>

          <!-- Modal with Validate -->
          <button class="btn btnPrimary" @click="modalValidate = !modalValidate">Form + validate</button>
          <modalValidate v-show="modalValidate" @close="modalValidate = false"/>

          <!-- Modal LogIn -->
          <button class="btn btnDefaul" @click="logIn = !logIn">LogIn</button>
          <logIn v-show="logIn" @close="logIn = false" @change="changeFormLoginRegister"/>

          <!-- Modal Registration -->
          <button class="btn btnDefaul" @click="register = !register">Create account</button>
          <register v-show="register" @close="register = false" @change="changeFormLoginRegister"/>

        </div>
      </section>
    </div>
  </div>
</template>

<script>
import modals from "@/components/UI/Modal.vue";
import modalValidate from "@/components/ModalValidate.vue";
import logIn from "@/components/LogIn.vue";
import register from "@/components/Registration.vue";
export default {
  components: { modals, modalValidate, logIn, register },
  data() {
    return {
      modalFirst: false,
      modalSecond: {
        show: false,
        name: "",
        email: "",
      },
      modalValidate: false,
      logIn: false,
      register: false,
    };
  },
  methods: {
    resetForm() {
      this.modalSecond.name = "";
      this.modalSecond.email = "";
      this.modalSecond.show = false;
    },
    submitSecondForm() {
      console.log({
        name: this.modalSecond.name,
        email: this.modalSecond.email,
      });
      this.resetForm();
    },
    modalSecondClose() {
      this.resetForm();
    },
    changeFormLoginRegister () {
      this.logIn = !this.logIn
      this.register = !this.register
    },
  },
};
</script>

<style>
</style>
