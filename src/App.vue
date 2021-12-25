<template>
  <div>
    <form @submit.prevent="checkForm" v-if="!formIsValid">
      <h2>Пациент</h2>
      <div class="person-info">
        <div class="left-block">
          <label for="lastName"
            >Фамилия*
            <input
              id="lastName"
              type="text"
              v-model="person.lastName"
              :class="v$.person.lastName.$error ? 'error' : ''"
            />
            <p
              class="error-message"
              v-show="v$.person.lastName.$error && v$.person.lastName.required"
            >
              Поле 'Фамилия' обязательно для заполнения
            </p>
          </label>
          <label for="name"
            >Имя*
            <input
              id="name"
              type="text"
              v-model="person.name"
              :class="v$.person.name.$error ? 'error' : ''"
            />
            <p
              class="error-message"
              v-show="v$.person.name.$error && v$.person.name.required"
            >
              Поле 'Имя' обязательно для заполнения
            </p>
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
            <p
              class="error-message"
              v-show="
                v$.person.birthday.required.$invalid &&
                v$.person.birthday.$error
              "
            >
              Поле 'Дата рождения' обязательно для заполнения. <br />
            </p>
            <p
              class="error-message"
              v-show="v$.person.birthday.$error && person.birthday"
            >
              Неккоректный формат даты.
            </p>
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
            <p
              class="error-message"
              v-show="
                v$.person.phoneNumber.$error && v$.person.phoneNumber.required
              "
            >
              Поле 'Номер телефона' обязательно для заполнения
            </p>
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
            <p
              class="error-message"
              v-show="
                v$.person.clientGroup.$error && v$.person.clientGroup.required
              "
            >
              Поле 'Группа клиентов' обязательно для заполнения
            </p>
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
      <h2>Адрес</h2>
      <div class="adress-info">
        <label for="index"
          >Индекс
          <input id="index" type="text" v-model="person.adress.index" />
        </label>
        <label for="country"
          >Страна
          <input id="country" type="text" v-model="person.adress.country" />
        </label>
        <label for="region"
          >Регион
          <input id="region" type="text" v-model="person.adress.region" />
        </label>
        <label for="city"
          >Город*
          <input
            id="city"
            type="text"
            v-model="person.adress.city"
            :class="v$.person.adress.city.$error ? 'error' : ''"
          />
          <p
            class="error-message"
            v-show="
              v$.person.adress.city.$error && v$.person.adress.city.required
            "
          >
            Поле 'Город' обязательно для заполнения
          </p>
        </label>
        <label for="street"
          >Улица
          <input id="street" type="text" v-model="person.adress.street" />
        </label>
        <label for="houseNumber"
          >Дом
          <input
            id="houseNumber"
            type="text"
            v-model="person.adress.houseNumber"
          />
        </label>
      </div>
      <h2>Документ</h2>

      <div class="document-info">
        <label for="document">
          Тип документа*
          <select
            id="document"
            type="text"
            v-model="person.document.type"
            :class="v$.person.document.type.$error ? 'error' : ''"
          >
            <option value="" disabled>Выберите документ:</option>
            <option
              v-for="(item, index) in documnetTypeSelect"
              :key="index"
              :value="item"
            >
              {{ item }}
            </option>
          </select>
          <p
            class="error-message"
            v-show="
              v$.person.document.type.$error && v$.person.document.type.required
            "
          >
            Поле 'Тип документа' обязательно для заполнения
          </p>
        </label>
        <div class="series-number">
          <label for="series"
            >Серия
            <input id="series" type="text" v-model="person.document.series" />
          </label>
          <label for="documentNumber"
            >Номер
            <input
              id="documentNumber"
              type="text"
              v-model="person.document.documentNumber"
            />
          </label>
        </div>
        <label for="issuedBy"
          >Кем выдан
          <input id="issuedBy" type="text" v-model="person.document.issuedBy" />
        </label>
        <label for="dateOfIssue"
          >Дата выдачи*
          <input
            id="dateOfIssue"
            type="date"
            v-model="person.document.dateOfIssue"
            :class="v$.person.document.dateOfIssue.$error ? 'error' : ''"
          />
          <p
            class="error-message"
            v-show="
              v$.person.document.dateOfIssue.required.$invalid &&
              v$.person.document.dateOfIssue.$error
            "
          >
            Поле 'Дата выдачи' обязательно для заполнения. <br />
          </p>
          <p
            class="error-message"
            v-show="
              v$.person.document.dateOfIssue.$error &&
              person.document.dateOfIssue
            "
          >
            Неккоректный формат даты.
          </p>
        </label>
      </div>
      <button>Отправить</button>
    </form>

    <div v-else class="congratulations">
      Регистрация прошла успешно
      <div>{{ this.person }}</div>
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
        adress: {
          index: '',
          city: '',
          street: '',
          country: '',
          region: '',
          houseNumber: '',
        },
        document: {
          type: '',
          series: '',
          documentNumber: '',
          issuedBy: '',
          dateOfIssue: '',
        },
      },
      formIsValid: false,
      clientGroupSelect: ['VIP', 'Проблемные', 'ОМС'],
      therapistSelect: ['Иванов', 'Захаров', 'Чернышева'],
      documnetTypeSelect: [
        'Паспорт',
        'Свидетельство о рождении',
        'Вод.удостоверение',
      ],
    };
  },

  validations: {
    person: {
      name: {
        required,
        alpha(value) {
          return /^[А-ЯЁ][а-яё]*$/.test(value);
        },
      },
      lastName: {
        required,
        alpha(value) {
          return /^[А-ЯЁ][а-яё]*$/.test(value);
        },
      },
      birthday: {
        required,
        dateValidation(value) {
          const date = new Date(value);
          const now = new Date();
          return date < now;
        },
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
      adress: {
        city: {
          required,
          alpha(value) {
            return /^[А-ЯЁ][а-яё]*$/.test(value);
          },
        },
      },
      document: {
        type: {
          required,
        },
        dateOfIssue: {
          required,
          dateValidation(value) {
            const date = new Date(value);
            const now = new Date();
            return date < now;
          },
        },
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
* {
  overflow-x: hidden;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 10px;
}
p {
  margin: 5px 0;
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
select {
  margin-bottom: 10px;
}
.person-info {
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
.adress-info {
  max-width: 700px;
  margin: 0 auto;
}

.document-info {
  max-width: 700px;
  margin: 0 auto;
  .series-number {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 50px;
  }
}
.error-messages {
  position: absolute;
  top: 10px;
  right: -1000px;
  animation: showError 15s;
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
    right: 10px;
  }
  10% {
    right: 10px;
  }
  20% {
    right: 10px;
  }
  30% {
    right: 10px;
  }
  40% {
    right: 10px;
  }
  100% {
    right: -1000px;
  }
}

@media (max-width: 500px) {
  .person-info {
    grid-template-columns: 1fr;
  }
}
</style>
