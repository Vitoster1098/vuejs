<template>
  <form class="sign-up" @submit.prevent="checkForm">
      <div class="form-group">
        <label for="login">Логин:</label>
        <input
          type="text"
          id="login"
          class="form-control"
          :class="$v.form.login.$error ? 'is-invalid' : ''"
          v-model.trim="form.login"
        >
          <p v-if="$v.form.login.$dirty && !$v.form.login.required" class="invalid-feedback">
              Обязательное поле
          </p>
          <p v-if="$v.form.login.$dirty && !$v.form.login.minLength" class="invalid-feedback">
              Здесь должно быть более 5-и символов
          </p>
      </div>
      <div class="form-group">
          <label for="email">Почта:</label>
          <input
            type="email"
            id="email"
            class="form-control"
            :class="$v.form.email.$error ? 'is-invalid' : ''"
            v-model.trim="form.email"
          >
          <p v-if="$v.form.email.$dirty && !$v.form.email.required" class="invalid-feedback">
              Обязательное поле
          </p>
          <p v-if="$v.form.email.$dirty && !$v.form.email.minLength" class="invalid-feedback">
              Здесь должно быть более 5-и символов
          </p>
      </div>
      <div class="form-group">
          <label for="password">Пароль:</label>
          <input
            type="password"
            id="password"
            class="form-control"
            :class="$v.form.password.$error ? 'is-invalid' : ''"
            v-model.trim="form.password"
          >
          <p v-if="$v.form.password.$dirty && !$v.form.password.required" class="invalid-feedback">
              Обязательное поле
          </p>
      </div>
      <div class="form-group">
          <label for="country">Страна проживания:</label>
          <select
              id="country"
              class="form-control"
              :class="$v.form.country.$error ? 'is-invalid' : ''"
              v-model="form.country"
          >
              <option
              v-for="(country, index) in countries"
              :value="country.value"
              :key="index"
              >
                  {{ country.label }}
              </option>
          </select>
          <p v-if="$v.form.country.$dirty && !$v.form.country.required" class="invalid-feedback">
              Обязательное поле
          </p>
      </div>
      <div class="form-group">
          <label for="themes">Любимые темы:</label>
          <select
              id="themes"
              class="form-control"
              :class="$v.form.theme.$error ? 'is-invalid' : ''"
              multiple
              v-model="form.theme"
          >
              <option
                  v-for="(theme, index) in themes"
                  :value="theme.value"
                  :key="index"
              >
                  {{ theme.label }}
              </option>
          </select>
          <p v-if="$v.form.theme.$dirty && !$v.form.theme.required" class="invalid-feedback">
              Обязательное поле
          </p>
          <p v-if="$v.form.theme.$dirty && !$v.form.theme.mustBeCool" class="invalid-feedback">
              Должно быть выбрано что-то и не более 3
          </p>
      </div>
      <div class="form-group form-check">
          <input
              type="checkbox"
              id="notification"
              class="form-check-input"
              :class="$v.form.agreeSendMail.$error ? 'is-invalid' : ''"
              value="1"
              v-model="form.agreeSendMail"
          >
          <p v-if="$v.form.agreeSendMail.$dirty && !$v.form.agreeSendMail.required" class="invalid-feedback">
              Обязательное поле
          </p>
          <label for="notification">Уведомлять меня о новых курсах</label>
      </div>
      <div class="form-group form-check">
          <input
              type="checkbox"
              id="processing"
              class="form-check-input"
              :class="$v.form.agreeProcessing.$error ? 'is-invalid' : ''"
              value="0"
              v-model="form.agreeProcessing"
          >
          <p v-if="$v.form.agreeProcessing.$dirty && !$v.form.agreeProcessing.required" class="invalid-feedback">
              Обязательное поле
          </p>
          <label for="notification">Согласен с обработкой данных</label>
      </div>
      <div class="flex">
          <div class="form-check">
              <input
                  type="radio"
                  id="male"
                  checked
                  class="form-check-input"
                  value="male"
                  name="exampleRadios"
                  v-model="form.gender"
              >
          </div>
          <label class="form-check-label" for="male">Мужчина</label>
          <div class="form-check">
              <input
                  type="radio"
                  id="female"
                  class="form-check-input"
                  value="female"
                  name="exampleRadios"
                  v-model="form.gender"
              >
          </div>
          <label class="form-check-label" for="female">Женщина</label>
      </div>
      <button type="submit" class="btn btn-primary">Сохранить</button>
  </form>
</template>

<script>
import { validationMixin} from "vuelidate/src";
import { required, minLength, email, sameAs } from 'vuelidate/lib/validators'

const mustBeCool = (value) => value.length > 0 && value.length <= 3

export default {
    mixins: [validationMixin],
    name: 'MyComponent',
    data() {
        return {
            form: {
                login: '',
                email: '',
                password: '',
                country: 'Russia',
                theme: ['IT'],
                agreeSendMail: false,
                agreeProcessing: false,
                gender: 'male'
            },
            countries: [
                  {
                      label: 'Россия',
                      value: 'Russia'
                  },
                  {
                      label: 'Америка',
                      value: 'USA'
                  },
                  {
                      label: 'Китай',
                      value: 'China'
                  }
            ],
            themes: [
                {
                    label: 'Технологии',
                    value: 'IT'
                },
                {
                    label: 'Языки',
                    value: 'Languages'
                },
                {
                    label: 'Искусство',
                    value: 'Art'
                }
            ]
        }
    },
    validations: {
        form: {
            login: {
                required, minLength: minLength(5)
            },
            email: {
                required, email
            },
            password: {
                required
            },
            theme: {
                mustBeCool, required
            },
            agreeProcessing:  {
                sameAs: sameAs( () => true )
            },
            country: {
                required
            },
            agreeSendMail: {
                sameAs: sameAs( () => true )
            }
        }
    },
    methods: {
        checkForm() {
            this.$v.form.$touch()
            if(this.$v.form.$error) {
                console.log('Валидация прошла успешно')
            }
        }
    }
}
</script>

<style></style>