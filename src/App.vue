<template>
  <div>
    <form @submit.prevent="checkForm" v-if="!formIsValid">
      <div class="person-main-info">
        <div class="left-block">
          <label for="lastName"
            >Фамилия*
            <input
              id="lastName"
              type="text"
              v-model="person.lastName"
              :class="v$.person.lastName.$error ? 'error' : ''"
            />
          </label>
          <label for="name"
            >Имя*
            <input
              id="name"
              type="text"
              v-model="person.name"
              :class="v$.person.name.$error ? 'error' : ''"
            />
          </label>

          <label for="secondName"
            >Отчество
            <input id="secondName" type="text" v-model="person.secondName" />
          </label>

          <label for="birthday"
            >Дата рождения*
            <input
              id="birthday"
              type="date"
              v-model="person.birthday"
              :class="v$.person.birthday.$error ? 'error' : ''"
            />
          </label>

          <label for="sex" class="sex"
            >Пол
            <input id="sex" type="radio" value="Мужской" v-model="person.sex" />
            Муж.
            <input id="sex" type="radio" value="Женский" v-model="person.sex" />
            Жен.
          </label>

          <label for="phoneNumber"
            >Номер телефона*
            <input
              id="phoneNumber"
              type="text"
              v-model="person.phoneNumber"
              :class="v$.person.phoneNumber.$error ? 'error' : ''"
            />
          </label>
        </div>
        <div class="right-block">
          <label for="clientGroup"
            >Группа клиентов*
            <select
              id="clientGroup"
              type="text"
              v-model="person.clientGroup"
              :class="v$.person.clientGroup.$error ? 'error' : ''"
              multiple
            >
              <option
                v-for="(item, index) in clientGroupSelect"
                :key="index"
                :value="item"
              >
                {{ item }}
              </option>
            </select>
          </label>

          <label for="therapist"
            >Лечащий врач
            <select id="therapist" type="text" v-model="person.therapist">
              <option value="" disabled>Выберите лечащего врача:</option>
              <option
                v-for="(item, index) in therapistSelect"
                :key="index"
                :value="item"
              >
                {{ item }}
              </option>
            </select>
          </label>
        </div>
        <label for="sendMessage" class="send-message"
          ><input
            id="sendMessage"
            type="checkbox"
            v-model="person.dontSendMessage"
          />
          Не отправлять СМС
        </label>
      </div>

      <button>Отправить</button>
    </form>
    <div v-else class="congratulations">
      Регистрация прошла успешно
      <div>{{ this.person }}</div>
    </div>
    <div class="error-messages">
      <p
        class="error-message"
        v-show="v$.person.lastName.$error && v$.person.lastName.required"
      >
        Поле 'Фамилия' обязательно для заполнения
      </p>
      <p
        class="error-message"
        v-show="v$.person.name.$error && v$.person.name.required"
      >
        Поле 'Имя' обязательно для заполнения
      </p>
      <!-- <p
        class="error-message"
        v-show="v$.person.name.$invalid && v$.person.name.required"
      >
        Поле 'Имя' должно состоять из Русских букв
      </p> -->
      <p
        class="error-message"
        v-show="v$.person.birthday.$error && v$.person.birthday.required"
      >
        Поле 'Дата рождения' обязательно для заполнения
      </p>
      <p
        class="error-message"
        v-show="v$.person.phoneNumber.$error && v$.person.phoneNumber.required"
      >
        Поле 'Номер телефона' обязательно для заполнения
      </p>
      <p
        class="error-message"
        v-show="v$.person.clientGroup.$error && v$.person.clientGroup.required"
      >
        Поле 'Группа клиентов' обязательно для заполнения
      </p>
    </div>
  </div>
</template>

<script>
import { required, minLength, maxLength } from '@vuelidate/validators';
import useVuelidate from '@vuelidate/core';

export default {
  name: 'App',
  setup: () => ({
    v$: useVuelidate(),
  }),
  data() {
    return {
      person: {
        name: '',
        lastName: '',
        secondName: '',
        birthday: '',
        phoneNumber: '+7',
        sex: '',
        clientGroup: [],
        therapist: '',
        dontSendMessage: false,
      },
      formIsValid: false,
      clientGroupSelect: ['VIP', 'Проблемные', 'ОМС'],
      therapistSelect: ['Иванов', 'Захаров', 'Чернышева'],
    };
  },

  validations: {
    person: {
      name: {
        required,
        fioValidation(value) {
          return /^[А-ЯЁ][а-яё]*$/.test(value);
        },
      },
      lastName: {
        required,
        fioValidation(value) {
          return /^[А-ЯЁ][а-яё]*$/.test(value);
        },
      },
      birthday: {
        required,
      },
      phoneNumber: {
        required,
        numberValidation(value) {
          return /[0-9]/.test(value);
        },
        minLength: minLength(12),
        maxLength: maxLength(12),
      },
      clientGroup: {
        required,
      },
    },
  },

  methods: {
    checkForm() {
      this.v$.person.$touch();
      if (this.v$.person.$error) {
        this.formIsValid = false;
      } else {
        this.formIsValid = true;
      }
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.invalid {
  box-shadow: 0px 0px 1px 2px red;
}
.person-main-info {
  max-width: 700px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: 1fr 1fr;
  column-gap: 50px;
  .sex,
  .send-message {
    display: flex;
    align-items: center;
  }
}
label {
  display: grid;
  text-align: left;
  margin-bottom: 10px;
}
input,
select {
  padding: 5px;
}
.error-messages {
  position: absolute;
  top: 10px;
  right: 10px;
}
.error-message {
  background-color: rgba(255, 0, 0, 0.7);
  color: white;
  border: 2px solid black;
  padding: 5px;
  border-radius: 4px;
  transition: 2s ease;
}
.error {
  border: 3px solid red;
}
.congratulations {
  background-color: rgb(1, 163, 1);
  color: white;
  padding: 20px;
  font-size: 2rem;
}
@keyframes showError {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

@media (max-width: 500px) {
  .person-main-info {
    grid-template-columns: 1fr;
  }
}
</style>
