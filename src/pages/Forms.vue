<template>
  <q-page class="q-ma-md">

    <span class="text-h3">Forms</span>
    <q-separator spaced />

    <div class="row justify-center">
      <q-form
        @submit="onSubmit"
        @reset="onReset"
        class="q-gutter-xs col-xs-12 col-sm-12 col-md-6 q-mt-xl"
    >
        <q-input
          filled
          v-model="userForm.email"
          label="Tu correo electronico"
          type="email"
          hint="Correo"
          lazy-rules
          :rules="[ 
            val => val && val.length > 0 || 'Este campo es obligatorio',
            isValidEmail
          ]"
        />

        <q-input
          filled
          type="password"
          v-model="userForm.password1"
          label="Password"
          lazy-rules
          :rules="[ val => val && val.length > 0 || 'Este campo es obligatorio']"
        />

        <q-input
          filled
          type="password"
          v-model="userForm.password2"
          label="Repetir password"
          lazy-rules
          :rules="[ 
            val => val && val.length > 0 || 'Este campo es obligatorio',
            isSamePassword
          ]"
        />

        <!-- <q-toggle v-model="accept" label="I accept the license and terms" /> -->

        <q-checkbox 
          v-model="userForm.conditions" 
          label="Acepto las condiciones y terminos de uso"
          :style="userForm.errorInCondicions 
            && !userForm.cunditions
            && 'color: red'" />

        <div class="row justify-end">
          <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
          <q-btn label="Submit" type="submit" color="primary"/>
        </div>
        </q-form>
    </div>

  </q-page>
</template>

<script>
import { defineComponent, ref } from 'vue'
import { useQuasar } from 'quasar'

export default defineComponent({
  name: 'Forms',

  setup() {

    const $q = useQuasar()

    const userForm = ref({
      email: '',
      password1: '',
      password2: '',
      conditions: false,
      errorInCondicions: false
    })

    
    return {
      userForm,

      onSubmit() {

        userForm.value.errorInCondicions = false

        if( !userForm.value.conditions ) {

          $q.notify({
            message: 'Debe aceptar los terminos y condiciones.',
            color: 'purple',
            icon: 'las la-exclamation-circle'
          })

          userForm.value.errorInCondicions = true
          return
        }

        console.log(userForm.value)
      },
      onReset() {
        userForm.value = {
          email: '',
          password1: '',
          password2: '',
          conditions: false,
          errorInCondicions: false
        }
      },

      isValidEmail( val ) {
        const emailPattern = /^(?=[a-zA-Z0-9@._%+-]{6,254}$)[a-zA-Z0-9._%+-]{1,64}@(?:[a-zA-Z0-9-]{1,63}\.){1,8}[a-zA-Z]{2,63}$/;
        return emailPattern.test(val) || 'El correo no parece ser válido';
      },

      isSamePassword( val ) {
        return ( val === userForm.value.password1) || 'Las contrasenas no son iguales'
      },

    }
  }
})
</script>
