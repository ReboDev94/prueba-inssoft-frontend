<template>
  <b-container fluid class="container__main">
    <b-row class="default__height">
      <b-col sm="12" lg="4" class="centered__items">
        <div class="card__default">
          <h4 class="text-center text-gray">Regístrate</h4>
          <p class="text-gray text-center">Ingresa tus datos para registrate</p>

          <b-alert
            v-model="configAlert.showAlert"
            :variant="configAlert.variant"
            dismissible
          >
            {{ configAlert.alertMessage }}
          </b-alert>

          <b-form class="mt-5" @submit="saveUser">
            <b-form-group id="name" label="Nombre:" label-for="input-name">
              <b-form-input
                id="input-name"
                v-model="formRegister.name"
                type="text"
                placeholder="Rafael De Jesús Rebolledo Hernández"
                required
              ></b-form-input>
            </b-form-group>

            <b-form-group
              id="name"
              label="Correo electrónico"
              label-for="input-email"
            >
              <b-form-input
                id="input-email"
                v-model="formRegister.email"
                type="email"
                placeholder="rrrrebolledohdz@gmail.com"
                required
              ></b-form-input>
            </b-form-group>

            <b-form-group
              id="name"
              label="Contraseña"
              label-for="input-password"
            >
              <b-input-group>
                <b-form-input
                  id="input-password"
                  v-model="formRegister.password"
                  :type="showPassword ? 'text' : 'password'"
                  required
                ></b-form-input>
                <template #append>
                  <b-button
                    variant="light"
                    class="border__eye__button"
                    type="button"
                    @click="showPassword = !showPassword"
                  >
                    <b-icon :icon="showPassword ? 'eye-slash' : 'eye'"></b-icon>
                  </b-button>
                </template>
              </b-input-group>
            </b-form-group>

            <b-form-group
              id="name"
              label="Confirmar contraseña"
              label-for="input-confirm-password"
            >
              <b-input-group>
                <b-form-input
                  id="input-confirm-password"
                  v-model="formRegister.confirmPassword"
                  :type="showConfirmPassword ? 'text' : 'password'"
                  required
                ></b-form-input>
                <template #append>
                  <b-button
                    variant="light"
                    class="border__eye__button"
                    type="button"
                    @click="showConfirmPassword = !showConfirmPassword"
                  >
                    <b-icon
                      :icon="showConfirmPassword ? 'eye-slash' : 'eye'"
                    ></b-icon>
                  </b-button>
                </template>
              </b-input-group>
            </b-form-group>

            <b-button type="submit" variant="primary" block>
              Registrate
            </b-button>
          </b-form>
        </div>
      </b-col>
      <b-col class="p-0">
        <b-img src="/portada.jpg" fluid class="h-100"></b-img>
      </b-col>
    </b-row>
  </b-container>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { RegisterResponse } from '@/interface/register.interfaces'

export default defineComponent({
  data() {
    return {
      configAlert: {
        showAlert: false,
        alertMessage: '',
        variant: 'success',
      },
      errors: [],
      showPassword: false,
      showConfirmPassword: false,
      formRegister: {
        name: '',
        email: '',
        password: '',
        confirmPassword: '',
      },
    }
  },
  methods: {
    async saveUser(event: Event) {
      event.preventDefault()
      this.errors = []
      await this.$axios
        .post<RegisterResponse>('user/register', this.formRegister)
        .then(() => {
          this.configAlert = {
            showAlert: true,
            alertMessage: 'El usuario ha sido registrado.',
            variant: 'success',
          }
          this.formRegister = {
            name: '',
            email: '',
            password: '',
            confirmPassword: '',
          }
        })
        .catch((error) => {
          this.errors = error.response.data.message
          if (this.errors.length > 0) {
            this.configAlert = {
              showAlert: true,
              alertMessage: this.errors[0],
              variant: 'danger',
            }
          }
        })
    },
  },
})
</script>
