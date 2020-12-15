<template>
  <div class="client">
    <form @submit.prevent="submitHandler">
      <div class="cap">
        <img 
          src="../assets/arrow-left.svg"
          alt="Назад"
          class="previousStep"
          @click="previousStep"
          v-if="step > 1 && step <4"
        >
        <div class="information">
          <h1 class="title" v-show="step === 1">Личные данные</h1>
          <h1 class="title" v-show="step === 2">Адрес</h1>
          <h1 class="title" v-show="step === 3">Паспорт</h1>
          <div class="steps">
            <span class="step" :class="{step_active: step === 1,step_past: step > 1}" v-show="step < 4"></span>
            <span class="step" :class="{step_active: step === 2,step_past: step > 2}" v-show="step < 4"></span>
            <span class="step" :class="{step_active: step === 3,step_past: step > 3}" v-show="step < 4"></span>
          </div>
        </div>
        <span
          class="nextStep"
          @click="btnNextStep1"
          v-show="step === 1"
        >Продолжить</span>
        <span
          class="nextStep"
          @click="btnNextStep2"
          v-show="step === 2"
        >Продолжить</span>
        <button
          type="submit"
          class="nextStep"
          v-show="step === 3"
        >Зарегистрироваться</button>
        <span
          class="nextStep"
          @click="toStart"
          v-show="step === 4"
        >В начало</span>
        <img 
          src="../assets/arrow-left.svg"
          alt="Продолжить"
          class="nextStepArrow"
          @click="btnNextStep1"
          v-show="step === 1"
        >
        <img 
          src="../assets/arrow-left.svg"
          alt="Продолжить"
          class="nextStepArrow"
          @click="btnNextStep2"
          v-show="step === 2"
        >
        <img 
          src="../assets/arrow-left.svg"
          alt="Продолжить"
          class="nextStepArrow"
          @click="submitHandler"
          v-show="step === 3"
        >
        <img 
          src="../assets/arrow-left.svg"
          alt="Продолжить"
          class="nextStepArrow"
          @click="toStart"
          v-show="step === 4"
        >
      </div>
      <div class="main">
        <div v-show="step === 1" class="personal-data">
          <div class="input-field">
            <label for="surname" class="input-field__label">Фамилия
              <span class="input-field__label_waring">*</span>
            </label>
            <input 
              v-model.trim="formReg.surname"
              :class="{invalid: statusInvalid(this.$v.formReg.surname) || !$v.formReg.surname.alphaWithDash, 
                       valid: statusAlphaWithDash(this.$v.formReg.surname) && statusValid(this.$v.formReg.surname)}"
              type="text" 
              id="surname"
            >
            <small v-if="statusInvalid(this.$v.formReg.surname)">{{ msgReq }}</small>
            <small v-else-if="!$v.formReg.surname.alphaWithDash">{{ msgAlph }}</small>
          </div>
          <div class="input-field">
            <label for="name" class="input-field__label">Имя
              <span class="input-field__label_waring">*</span>
            </label>
            <input 
              v-model.trim="formReg.name" 
              :class="{invalid: statusInvalid(this.$v.formReg.name) || !$v.formReg.name.alphaWithDash,
                       valid: statusAlphaWithDash(this.$v.formReg.name) && statusValid(this.$v.formReg.name)}"
              type="text"
              id="name"
            >
            <small v-if="statusInvalid(this.$v.formReg.name)">{{ msgReq }}</small>
            <small v-else-if="!$v.formReg.name.alphaWithDash">{{ msgAlph }}</small>
          </div>
          <div class="input-field">
            <label for="patronymic" class="input-field__label">Отчество</label>
            <input 
              v-model.trim="formReg.patronymic"
              :class="{invalid: !$v.formReg.patronymic.alphaWithDash}"
              type="text" 
              id="patronymic"
            >
            <small v-if="!$v.formReg.patronymic.alphaWithDash">{{ msgAlph }}</small>
          </div>
          <div class="input-field">
            <label for="dateOfBirth" class="input-field__label">Дата рождения
              <span class="input-field__label_waring">*</span>
            </label>
            <input 
              v-model.trim="formReg.dateOfBirth"
              :class="{invalid: statusInvalid(this.$v.formReg.dateOfBirth) || !$v.formReg.dateOfBirth.numericWithDash,
                       valid: statusNumericWithDot(this.$v.formReg.dateOfBirth) && statusValid(this.$v.formReg.dateOfBirth)}"
              type="text"
              onfocus="(this.type='date')"
              onblur="if(!this.value)this.type='text'"
              id="dateOfBirth"
            >
            <small v-if="statusInvalid(this.$v.formReg.dateOfBirth)">{{ msgReq }}</small>
            <small v-else-if="!$v.formReg.dateOfBirth.numericWithDash">{{ msgNum }}</small>
          </div>
          <div class="input-field">
            <label for="phoneNumber" class="input-field__label">Номер телефона
              <span class="input-field__label_waring">*</span>
            </label>
            <the-mask 
              v-model.trim="formReg.phoneNumber"
              :class="{invalid: statusInvalid(this.$v.formReg.phoneNumber)
                       || ($v.formReg.phoneNumber.$dirty  && !$v.formReg.phoneNumber.minLength)
                       || ($v.formReg.phoneNumber.$dirty  && !$v.formReg.phoneNumber.maxLength)
                       || !$v.formReg.phoneNumber.numeric,
                       valid: statusPhoneNumber(this.$v.formReg.phoneNumber) && statusValid(this.$v.formReg.phoneNumber)}"
              type="tel"
              id="phoneNumber"
              :mask="['+7 (###) ###-##-##']"
            />
            <small v-if="statusInvalid(this.$v.formReg.phoneNumber)">{{ msgReq }}</small>
            <small v-else-if="!$v.formReg.phoneNumber.minLength"> Телефоный номер должен состоять из 11 цифр.</small>
            <small v-else-if="!$v.formReg.phoneNumber.maxLength"> Телефоный номер не должен превышать 11 цифр.</small>
            <small v-else-if="!$v.formReg.phoneNumber.numeric">{{ msgNum }}</small>
          </div>
          <div class="input-field">
            <label for="gender" class="input-field__label">Ваш пол:</label>
            <multiselect 
              v-model.trim="formReg.gender" 
              :options="gender_options" 
              :searchable="false" 
              :close-on-select="true" 
              :show-labels="false" 
              placeholder=""
              id="gender"
            ></multiselect>
          </div>
          <div class="input-field">
            <label for="group" class="input-field__label">Ваша(и) группа(ы)
              <span class="input-field__label_waring">*</span>:
            </label>
            <multiselect 
              v-model.trim="formReg.group"
              :class="{invalid: statusInvalid(this.$v.formReg.group),
                       valid: statusValid(this.$v.formReg.group)}"
              selectLabel="Нажмите Enter, чтобы выбрать"
              deselectLabel="Нажмите Enter, чтобы удалить"
              selectedLabel="Выбрано"
              label="сlient_group"
              track-by="сlient_group"
              :options="group_options"
              :multiple="true"
              :taggable="true"
              @tag="addTag"
              placeholder=""
            ></multiselect>
            <small v-if="statusInvalid(this.$v.formReg.group)">{{ msgReq }}</small>
          </div>
          <div class="input-field">
            <label for="doctor" class="input-field__label">Ваш лечащий врач:</label>
            <multiselect 
              v-model.trim="formReg.doctor" 
              :options="doctor_options" 
              :searchable="false" 
              :close-on-select="true" 
              :show-labels="false"
              placeholder=""
            ></multiselect>
          </div>
          <div class="input-field input-field_last">
            <label for="sms" class="input-field__label">Не отправлять СМС</label>
            <input v-model="formReg.sms" type="checkbox" id="sms">
          </div>
        </div>
        <div v-show="step === 2" class="personal-data">
          <div class="input-field">
            <label for="index" class="input-field__label">Ваш индекс</label>
            <input 
              v-model.trim="formReg.index"
              :class="{invalid: !$v.formReg.index.numeric}"
              type="text" 
              id="index"
            >
            <small v-if="!$v.formReg.index.numeric">{{ msgNum }}</small>
          </div>
          <div class="input-field">
            <label for="country" class="input-field__label">Ваша страна</label>
            <input
              v-model.trim="formReg.country"
              :class="{invalid: !$v.formReg.country.alphaWithDash}"
              type="text" 
              id="country"
            >
            <small v-if="!$v.formReg.country.alphaWithDash">{{ msgAlph }}</small>
          </div>
          <div class="input-field">
            <label for="region" class="input-field__label">Ваша область</label>
            <input 
              v-model.trim="formReg.region"
              :class="{invalid: !$v.formReg.region.alphaRu}"
              type="text"
              id="region"
            >
            <small v-if="!$v.formReg.region.alphaRu">{{ msgAlph }}</small>
          </div>
          <div class="input-field">
            <label for="city" class="input-field__label">Ваш город
              <span class="input-field__label_waring">*</span>
            </label>
            <input 
              v-model.trim="formReg.city" 
              :class="{invalid: statusInvalid(this.$v.formReg.city) || !$v.formReg.city.alphaWithDash,
                       valid: statusAlphaWithDash(this.$v.formReg.city) && statusValid(this.$v.formReg.city)}"
              type="text" 
              id="city"
            >
            <small v-if="statusInvalid(this.$v.formReg.city)">{{ msgReq }}</small>
            <small v-else-if="!$v.formReg.city.alphaWithDash">{{ msgAlph }}</small>
          </div>
          <div class="input-field">
            <label for="street" class="input-field__label">Ваша улица</label>
            <input 
              v-model.trim="formReg.street"
              :class="{invalid: !$v.formReg.street.alphaWithDash}"
              type="text"
              id="street"
            >
            <small v-if="!$v.formReg.street.alphaWithDash">{{ msgAlph }}</small>
          </div>
          <div class="input-field">
            <label for="house" class="input-field__label">Ваш дом</label>
            <input
              v-model.trim="formReg.house"
              :class="{invalid: !$v.formReg.house.alphaNumeric}"
              type="text"
              id="house"
            >
            <small v-if="!$v.formReg.house.alphaNumeric">{{ msgAlphNum }}</small>
          </div>
        </div>
        <div v-show="step === 3" class="personal-data">
          <div class="input-field">
            <label for="document" class="input-field__label">Тип документа
              <span class="input-field__label_waring">*</span>
            </label>
            <multiselect 
              v-model.trim="formReg.document" 
              :options="document_options" 
              :searchable="false" 
              :close-on-select="true" 
              :show-labels="false"
              :class="{invalid: statusInvalid(this.$v.formReg.document),
                       valid: statusValid(this.$v.formReg.document)}"
              id="document"
              placeholder=""
            ></multiselect>
            <small v-if="statusInvalid(this.$v.formReg.document)">{{ msgReq }}</small>  
          </div>
          <div class="input-field">
            <label for="series" class="input-field__label">Серия</label>
            <input
              v-model="formReg.series"
              :class="{invalid: !$v.formReg.series.numeric}"
              type="text"
              id="series"
            >
            <small v-if="!$v.formReg.series.numeric">{{ msgNum }}</small>
          </div>
          <div class="input-field">
            <label for="documentNumber" class="input-field__label">Номер</label>
            <input
              v-model="formReg.documentNumber"
              :class="{invalid: !$v.formReg.documentNumber.numeric}"
              type="text"
              id="documentNumber"
            >
            <small v-if="!$v.formReg.documentNumber.numeric">{{ msgNum }}</small>
          </div>
          <div class="input-field">
            <label for="issuedBy" class="input-field__label">Кем выдан</label>
            <input
              v-model.trim="formReg.issuedBy"
              :class="{invalid: !$v.formReg.issuedBy.alphaNumeric}"
              type="text"
              id="issuedBy"
            >
            <small v-if="!$v.formReg.issuedBy.alphaNumeric">{{ msgAlphNum }}</small>
          </div>
          <div class="input-field">
            <label for="dateOfIssue" class="input-field__label">Дата выдачи
              <span class="input-field__label_waring">*</span>
            </label>
            <input 
              v-model.trim="formReg.dateOfIssue" 
              :class="{invalid: statusInvalid(this.$v.formReg.dateOfIssue) || !$v.formReg.dateOfIssue.numericWithDash,
                       valid: statusNumericWithDot(this.$v.formReg.dateOfIssue) && statusValid(this.$v.formReg.dateOfIssue)}"
              type="text"
              onfocus="(this.type='date')"
              onblur="if(!this.value)this.type='text'"
              id="dateOfIssue"  
            >
            <small v-if="statusInvalid(this.$v.formReg.dateOfIssue)">{{ msgReq }}</small>  
            <small v-if="!$v.formReg.dateOfIssue.numericWithDash">{{ msgNum }}</small>
          </div>
        </div>
        <div v-show="step === 4" class="success">
          <h1>Клиент успешно создан</h1>
        </div>
        <div v-show="step < 4" class="warning">
          <p><span>*</span> Поле обязательное для заполнения</p>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import Multiselect from 'vue-multiselect'
import {required, minLength, maxLength, alpha, helpers, alphaNum, numeric} from 'vuelidate/lib/validators'
import {TheMask} from 'vue-the-mask'


const alphaRu = helpers.regex('alpha', /^[a-zA-Zа-яА-ЯёЁ]*$/)
const alphaWithDash = helpers.regex('alpha', /^[a-zA-Zа-яА-ЯёЁ-]*$/)
const numericWithDash = helpers.regex('numeric', /^[0-9-]*$/)
const alphaNumeric = helpers.regex('alphaNum', /^[a-zA-Zа-яА-ЯёЁ0-9/.-\s]*$/)
alphaNum
alpha

export default {
  name: 'NewClient',
  components: {
    Multiselect,
    TheMask
  },
  data() {
    return {
      step: 1,
      gender_options: ['Мужской', 'Женский'],
      group_options: [
        { сlient_group: 'VIP'},
        { сlient_group: 'Проблемные'},
        { сlient_group: 'ОМС'}
      ],
      doctor_options: ['Иванов', 'Захаров', 'Чернышева'],
      document_options: ['Паспорт', 'Свидетельство о рождении', 'Вод. удостоверение'],
      formReg: {
        surname: '',
        name: '',
        patronymic: '',
        dateOfBirth: '',
        phoneNumber: '',
        gender: '',
        group: '',
        doctor: '',
        sms: '',
        index: '',
        country: '',
        region: '',
        city: '',
        street: '',
        house: '',
        document: '',
        series: '',
        documentNumber: '',
        issuedBy: '',
        dateOfIssue: ''
      },
      msgAlph: ' В поле не должно содержаться цифр.',
      msgNum: ' В поле не должно содержаться букв.',
      msgAlphNum: ' Разрешены буквы, цифры и символы: .-/',
      msgReq: ' Поле обязательно для заполнения.'
    }
  },
  validations: {
    formReg: {
      surname: {required, alphaWithDash},
      name: {required, alphaWithDash},
      patronymic: {alphaWithDash},
      dateOfBirth: {required, numericWithDash},
      phoneNumber: {required, minLength: minLength(10), maxLength: maxLength(10), numeric},
      gender: {},
      group: {required},
      doctor: {},
      sms: {},
      index: {numeric},
      country: {alphaWithDash},
      region: {alphaRu},
      city: {required, alphaWithDash},
      street: {alphaWithDash},
      house: {alphaNumeric},
      document: {required},
      series: {numeric},
      documentNumber: {numeric},
      issuedBy: {alphaNumeric},
      dateOfIssue: {required, numericWithDash}
    }
  },
  methods: {
    btnNextStep1 () {
      if (this.step === 1) {
        if (this.$v.formReg.surname.$invalid ||
             this.$v.formReg.name.$invalid ||
             this.$v.formReg.dateOfBirth.$invalid ||
             this.$v.formReg.phoneNumber.$invalid ||
             this.$v.formReg.group.$invalid) {
          this.$v.formReg.surname.$touch()
          this.$v.formReg.name.$touch()
          this.$v.formReg.dateOfBirth.$touch()
          this.$v.formReg.phoneNumber.$touch()
          this.$v.formReg.group.$touch()
        } else {
          return this.step++
        }
      }
    },
    btnNextStep2 () {
      if (this.step === 2) {
        if (this.$v.formReg.city.$invalid) {
          this.$v.formReg.city.$touch()
          return 
        } else {
          return this.step++
        }
      }
    },
    submitHandler () {
      if (this.$v.formReg.document.$invalid,
          this.$v.formReg.dateOfIssue.$invalid) {
        this.$v.formReg.document.$touch()
        this.$v.formReg.dateOfIssue.$touch()
        return
      } else {
        this.formReg.phoneNumber = '7' + this.formReg.phoneNumber
        const formData = {
          surname: this.formReg.surname,
          name: this.formReg.name,
          patronymic: this.formReg.patronymic,
          dateOfBirth: this.formReg.dateOfBirth,
          phoneNumber: this.formReg.phoneNumber,
          gender: this.formReg.gender,
          group: this.formReg.group,
          doctor: this.formReg.doctor,
          sms: this.formReg.sms,
          index: this.formReg.index,
          country: this.formReg.country,
          region: this.formReg.region,
          city: this.formReg.city,
          street: this.formReg.street,
          house: this.formReg.house,
          document: this.formReg.document,
          series: this.formReg.series,
          documentNumber: this.formReg.documentNumber,
          issuedBy: this.formReg.issuedBy,
          dateOfIssue: this.formReg.dateOfIssue
        }
        console.log(formData)
        this.step++
      }
    },
    toStart () {
      this.step = 1
      this.formReg = {
        surname: '',
        name: '',
        patronymic: '',
        dateOfBirth: '',
        phoneNumber: '',
        gender: '',
        group: '',
        doctor: '',
        sms: '',
        index: '',
        country: '',
        region: '',
        city: '',
        street: '',
        house: '',
        document: '',
        series: '',
        documentNumber: '',
        issuedBy: '',
        dateOfIssue: ''
      }
      this.$v.$reset()
    },
    previousStep () {
      if (this.step > 1) {
        this.step--
      }
    },
    addTag (newTag) {
      const tag = {
        сlient_group: newTag
      }
      this.group_options.push(tag)
      this.group.push(tag)
    },
    statusInvalid (validator) {
      if ((validator.$dirty && !validator.required) === true) {
        return true
      } else {
        return false
      }
    },
    statusValid (validator) {
      if ((validator.$dirty && validator.required) === true) {
        return true
      } else {
        return false
      }
    },
    statusAlphaWithDash (validator) {
      if ((validator.$dirty && validator.alphaWithDash) === true) {
        return true
      } else {
        return false
      }
    },
    statusNumericWithDot (validator) {
      if ((validator.$dirty && validator.numericWithDash) === true) {
        return true
      } else {
        return false
      }
    },
    statusPhoneNumber (validator) {
      if ((validator.$dirty && validator.numeric && validator.minLength && validator.maxLength) === true) {
        return true
      } else {
        return false
      }
    }
  }
}
</script>

<style lang="scss">
.cap {
  position: fixed;
  top: 0;
  width: 100%;
  height: 56px;
  background-color: #FFFFFF;
  box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.08);
  z-index: 1000;
  .previousStep {
    position: relative;
    top: 25%;
    bottom: 25%;
    left: 10%;
    cursor: pointer;
  }
  .title {
    font-weight: 500;
    font-size: 20px;
    line-height: 24px;
    margin: 0;
    position: absolute;
    top: 8px;
    left: 50%;
    transform: translateX(-50%);
  }
  .steps {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    padding: 0px;

    position: absolute;
    width: 208px;
    height: 4px;
    left: calc(50% - 208px/2);
    top: 40px;
    .step {
      display: inline-block;
      width: 64px;
      height: 4px;
      background-color: #EEEEEE;
      border-radius: 2px;
      &:nth-child(2) {
        margin: 0 8px;
      }
      &_active {
        background-color: #6699CC;
      }
      &_past {
        background-color: #C4DCF5;
      }
    }
  }
  .nextStep {
    position: absolute;
    top: calc(50% - 40px/2);
    right: 10%;
    cursor: pointer;
    color: #ffffff;
    font-size: 16px;
    line-height: 20px;
    padding: 10px 12px;
    border: unset;
    border-radius: 8px;
    background-color: #6699CC;
  }
  .nextStepArrow {
    display: none;
    position: absolute;
    top: 25%;
    right: 5%;
    transform: rotate(180deg);
    cursor: pointer;
  }
}
.main {
  margin-top: 72px;
  .personal-data {
    width: 100%;
    .input-field {
      display: grid;
      margin: 0 auto 8px auto;
      width: 37.5%;
      &__label {
        margin: 0 0 4px 12px;
        font-size: 12px;
        line-height: 16px;
        &_waring {
          color: #CC3333;
        }
      }
      input {
        border: unset;
        box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.08);
        border-radius: 8px;
        font-family: Roboto;
        font-style: normal;
        font-weight: normal;
        font-size: 16px;
        line-height: 20px;
        color: #000000;
        padding: 10px 0 10px 12px;
        &:focus, &:hover {
          box-shadow: 0px 8px 12px rgba(0, 0, 0, 0.08);
        }
      }
      small {
        font-size: 12px;
        line-height: 16px;
        color: #CC3333;
        margin: 4px 0 0 12px;
      }
      .invalid {
        border: 1px solid #CC3333;
      }
      .valid {
        border: 1px solid #6699CC;
      }
      .multiselect {
        border-radius: 8px;
        &__tags {
          border: unset;
          border-radius: 8px;
          box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.08);
          &:focus, &:hover {
            box-shadow: 0px 8px 12px rgba(0, 0, 0, 0.08);
          }
        }
        &__tag {
          background: #6699CC;
          &-icon {
            &::after {
              color: #607D99;
            }
            &:focus, &:hover {
              background: #215485;
              &::after {
                color: #FFFFFF;
              }
            }
          }
        }
        &__content-wrapper {
          border: unset;
          border-radius: 8px;
        }
        &__option--highlight {
          background: #6699CC;
          &::after {
            background: #6699CC;
          }
        }
        &__option--selected.multiselect__option--highlight {
          background: #CC3333;
          &::after {
            background: #CC3333;
          }
        }
      }
      &_last {
        display: flex;
        input {
          margin: 0 0 0 4px;
          height: 16px;

        }
      }
    }
  }
  .success {
    color: #6699CC;
    text-align: center;
  }
  .warning {
    width: 37.5%;
    margin: auto;
    p {
      margin: 0 0 8px 0;
      text-align: end;
      font-size: 12px;
      line-height: 16px;
      span {
        color: #CC3333;
      }
    }
  }
}
@media all and (max-width: 768px) {
  .cap {
    .title {
      font-size: 16px;
    }
    .steps{
      .step {
        width: 48px;
      }
    }
    .nextStep {
      font-size: 12px;
    }
  }
  .main {
    .personal-data {
      .input-field {
        width: 50%;
      }
    }
    .warning {
      width: 50%;
    }
  }
}
@media all and (max-width: 576px) {
  .cap {
    .previousStep {
      left: 5%;
    }
    .steps{
      .step {
        width: 16px;
      }
    }
    .nextStep {
      display: none;
    }
    .nextStepArrow {
      display: flex;
    }
  }
  .main {
    .personal-data {
      .input-field {
        width: 75%;
      }
    }
    .warning {
      width: 75%;
    }
    .success {
      h1 {
        font-size: 20px;
      }
    }
  }
}
</style>
