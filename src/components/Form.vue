<template>
  <form class="root" @submit.prevent="submitForm">
    <h2>Create an Account</h2>
    <p>
      <input type="text" placeholder="Email" v-model="state.email"/>
      <span v-if="v$.email.$error">{{ v$.email.$errors[0].$message }}</span>
    </p>
    <p>
      <input type="password" placeholder="Password" v-model="state.password.password"/>
      <span v-if="v$.password.password.$error">{{ v$.password.password.$errors[0].$message }}</span>
    </p>
    <p>
      <input type="password" placeholder="Confirm Password" v-model="state.password.confirmPassword"/>
      <span v-if="v$.password.confirmPassword.$error">{{ v$.password.confirmPassword.$errors[0].$message }}</span>
    </p>
    <input type="submit" value="Submit">
  </form>
</template>


<script>
import useValidate from '@vuelidate/core'
import {required, email, minLength, sameAs, helpers} from '@vuelidate/validators'
import {reactive, computed} from "vue";

export default {
  name: "Form",
  setup() {
    const state = reactive({
      email: '',
      password: {
        password: '',
        confirmPassword: ''
      }
    })

    const debeSerMohamed = (value) => value.includes('mohamed')

    const rules = computed(() => {
      return {
        email: {
          required,
          email: helpers.withMessage(
              'Debe ser un email valido',
              email
          ),
          debeSerMohamed: helpers.withMessage(
              'Debe contener Mohamed',
              debeSerMohamed
          )
        },
        password: {
          password: {required, minLength: minLength(6)},
          confirmPassword: {required, sameAs: sameAs(state.password.password)}
        }
      }
    })

    const v$ = useValidate(rules, state)

    return {
      state,
      v$
    }
  },
  methods: {
    submitForm() {
      this.v$.$validate()
      if (!this.v$.$error) {
        alert('Form enviado correctamente')
      } else {
        alert('Error de validacion')
      }
    }
  }
}
</script>

<style scoped>
.root {
  width: 400px;
  margin: 0 auto;
  background-color: #fff;
  padding: 30px;
  margin-top: 100px;
  border-radius: 20px;
}

input {
  border: none;
  outline: none;
  border-bottom: 1px solid #ddd;
  font-size: 1em;
  padding: 5px 0;
  margin: 10px 0 5px 0;
  width: 100%;
}

input[type="submit"] {
  background-color: #3498db;
  padding: 10px 20px;
  margin-top: 10px;
  border: none;
  color: white;
}
</style>