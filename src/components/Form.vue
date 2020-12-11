<template>
   <section>
      <form class="m-auto" @submit.prevent="submitHandler">
         <div class="d-flex justify-between sm-flex-column">
            <div class="input-field">
               <label for="lastname">Фамилия*</label>
               <input
                  class="form-control"
                  v-model.trim="lastname"
                  :class="{ error: $v.lastname.$dirty && !$v.lastname.required }"
                  type="text"
                  id="lastname"
               />
               <div class="error" v-if="$v.lastname.$dirty && !$v.lastname.required">
                  Поле Фамилия не должно быть пустым
               </div>
            </div>
            <div class="input-field">
               <label for="name">Имя*</label>
               <input
                  class="form-control"
                  v-model.trim="name"
                  :class="{ error: $v.name.$dirty && !$v.name.required }"
                  type="text"
                  id="name"
               />
               <div class="error" v-if="$v.name.$dirty && !$v.name.required">
                  Поле Имя не должно быть пустым
               </div>
            </div>
         </div>
         <div class="d-flex justify-between sm-flex-column">
            <div class="input-field">
               <label for="fatherName">Отчество</label>
               <input class="form-control" v-model.trim="fatherName" type="text" id="fatherName" />
            </div>
            <div class="input-field">
               <span>Пол</span>
               <div class="d-flex flex-column">
                  <label for="man"
                     ><input
                        v-model="sex"
                        type="radio"
                        id="man"
                        name="sex"
                        value="man"
                     />Мужчина</label
                  >
                  <label for="woman"
                     ><input
                        v-model="sex"
                        type="radio"
                        id="woman"
                        name="sex"
                        value="woman"
                     />Женщина</label
                  >
               </div>
            </div>
         </div>
         <div class="d-flex justify-between sm-flex-column">
            <div class="input-field">
               <label for="birthday">Дата рождения*</label>
               <input
                  class="form-control"
                  v-model="birthday"
                  :class="{ error: $v.birthday.$dirty && !$v.birthday.required }"
                  type="date"
                  id="birthday"
               />
               <div class="error" v-if="$v.birthday.$dirty && !$v.birthday.required">
                  Поле Дата рождения не должно быть пустым
               </div>
            </div>
            <div class="input-field">
               <label for="tel">Номер телефона*</label>
               <input
                  class="form-control"
                  id="tel"
                  maxlength="11"
                  placeholder="7(ххх)ххх-хххх"
                  v-model.trim="tel"
                  @focus="setMask($event)"
                  type="tel"
               />

               <div class="error" v-if="$v.tel.$dirty && !$v.tel.required">
                  Поле Телефон не должно быть пустым
               </div>
               <div class="error" v-else-if="!$v.tel.minLength">
                  Телефон должен быть {{ $v.tel.$params.minLength.min }} символов. Сейчас он
                  {{ tel.length }}
               </div>
            </div>
         </div>
         <div class="d-flex justify-between sm-flex-column">
            <div class="input-field">
               <label for="client">Группа клиентов*</label>
               <select
                  v-model="choosenClient"
                  :class="{
                     error: $v.choosenClient.$dirty && !$v.choosenClient.required,
                  }"
                  class="form-control"
                  name="client"
                  id="client"
                  multiple
               >
                  <option v-for="(client, index) in clients" :value="client.value" :key="index">
                     {{ client.label }}
                  </option>
               </select>
               <div
                  class="error"
                  v-if="
                     ($v.choosenClient.$dirty && !$v.choosenClient.required) ||
                        ($v.choosenClient.$dirty && choosenClient.length === 0)
                  "
               >
                  Поле Группа клиентов не должно быть пустым
               </div>
            </div>
            <div class="input-field">
               <label for="doctor">Лечащий врач</label>
               <select v-model="choosenDoctor" class="form-control" name="doctor" id="doctor">
                  <option
                     v-for="(doctor, index) in doctors"
                     :selected="doctor.selected"
                     :disabled="doctor.disabled"
                     :hidden="doctor.hidden"
                     :value="doctor.value"
                     :key="index"
                  >
                     {{ doctor.label }}
                  </option>
               </select>
            </div>
         </div>
         <div class="d-flex justify-between sm-flex-column">
            <div class="input-field flex-row">
               <input
                  v-model="notSendSMS"
                  type="checkbox"
                  id="notSendSMS"
                  name="subscribe"
                  value="notSendSMS"
               />
               <label for="notSendSMS">Не отправлять СМС</label>
            </div>
         </div>
         <h4>Адрес:</h4>
         <div class="d-flex justify-between sm-flex-column">
            <div class="input-field">
               <label for="index">Индекс</label>
               <input class="form-control" v-model.trim="index" type="text" id="index" />
            </div>
            <div class="input-field">
               <label for="country">Страна</label>
               <input class="form-control" v-model.trim="country" type="text" id="country" />
            </div>
         </div>
         <div class="d-flex justify-between sm-flex-column">
            <div class="input-field">
               <label for="region">Область</label>
               <input class="form-control" v-model.trim="region" type="text" id="region" />
            </div>
            <div class="input-field">
               <label for="city">Город*</label>
               <input class="form-control" v-model="city" type="text" id="city" />
               <div class="error" v-if="$v.city.$dirty && !$v.city.required">
                  Поле Город не должно быть пустым
               </div>
            </div>
         </div>
         <div class="d-flex justify-between sm-flex-column">
            <div class="input-field">
               <label for="street">Улица</label>
               <input class="form-control" v-model="street" type="text" id="street" />
            </div>
            <div class="input-field">
               <label for="house">Дом</label>
               <input class="form-control" v-model.trim="house" type="text" id="house" />
            </div>
         </div>
         <h4>Паспорт:</h4>
         <div class="d-flex justify-between sm-flex-column">
            <div class="input-field">
               <label for="document">Тип документа*</label>
               <select v-model="choosenDocument" class="form-control" name="document" id="document">
                  <option
                     v-for="(document, index) in documents"
                     :selected="document.selected"
                     :disabled="document.disabled"
                     :hidden="document.hidden"
                     :value="document.value"
                     :key="index"
                  >
                     {{ document.label }}
                  </option>
               </select>
               <div class="error" v-if="$v.choosenDocument.$dirty && !$v.choosenDocument.required">
                  Поле Тип документа не должно быть пустым
               </div>
            </div>
            <div class="input-field">
               <label for="series">Серия</label>
               <input class="form-control" v-model.trim="series" type="text" id="series" />
            </div>
         </div>
         <div class="d-flex justify-between sm-flex-column">
            <div class="input-field">
               <label for="number">Номер</label>
               <input class="form-control" v-model.trim="number" type="text" id="number" />
            </div>
            <div class="input-field">
               <label for="issuedBy">Кем выдан</label>
               <input class="form-control" v-model="issuedBy" type="text" id="issuedBy" />
            </div>
         </div>
         <div class="d-flex justify-between sm-flex-column">
            <div class="input-field">
               <label for="dateOfIssue">Дата выдачи*</label>
               <input class="form-control" v-model="dateOfIssue" type="date" id="dateOfIssue" />
               <div class="error" v-if="$v.dateOfIssue.$dirty && !$v.dateOfIssue.required">
                  Поле Дата выдачи не должно быть пустым
               </div>
            </div>
         </div>
         <h5>* Поле обязательное для заполнения.</h5>
         <button class="button" type="submit">Submit!</button>
      </form>
      <Registered v-if="showModal" @close="clearData()" :lastName="lastname" :name="name" />
   </section>
</template>

<script>
import { required, minLength, maxLength } from 'vuelidate/lib/validators';
import Registered from './Registered';
export default {
   name: 'Form',
   props: {},
   components: { Registered },
   data: () => ({
      showModal: false,
      lastname: '',
      name: '',
      fatherName: '',
      birthday: '',
      tel: '',
      sex: '',
      choosenClient: [],
      clients: [
         { value: 'vip', label: 'VIP' },
         { value: 'problem', label: 'Проблемные' },
         { value: 'omc', label: 'ОМС' },
      ],
      choosenDoctor: '',
      doctors: [
         {
            value: '',
            selected: 'selected',
            disabled: 'disabled',
            hidden: 'hidden',
            label: 'Выберите врача',
         },
         { value: 'ivanov', label: 'Иванов' },
         { value: 'zaharov', label: 'Захаров' },
         { value: 'chernishov', label: 'Чернышева' },
      ],
      notSendSMS: false,
      index: '',
      country: '',
      region: '',
      city: '',
      street: '',
      house: '',
      choosenDocument: '',
      documents: [
         {
            value: '',
            selected: 'selected',
            disabled: 'disabled',
            hidden: 'hidden',
            label: 'Выберите тип документа',
         },
         { value: 'passport', label: 'Паспорт' },
         { value: 'birthCertificate', label: 'Свидетельство о рождении' },
         { value: 'driverLicense', label: 'Вод. удостоверение' },
      ],
      series: '',
      number: '',
      issuedBy: '',
      dateOfIssue: '',
   }),
   validations: {
      lastname: { required },
      name: { required },
      birthday: { required },
      tel: { required, maxLength: maxLength(11), minLength: minLength(11) },
      choosenClient: { required },
      city: { required },
      choosenDocument: { required },
      dateOfIssue: { required },
   },
   watch: {
      showModal: function() {
         document.body.style.overflow = this.showModal ? 'hidden' : '';
      },
   },
   methods: {
      setMask(event) {
         if (event.target.value.length === 0) {
            this.tel = '7';
         }
      },
      clearData() {
         (this.showModal = false),
            (this.lastname = ''),
            (this.name = ''),
            (this.fatherName = ''),
            (this.birthday = ''),
            (this.tel = ''),
            (this.sex = ''),
            (this.choosenClient = []),
            (this.choosenDoctor = ''),
            (this.notSendSMS = false),
            (this.index = ''),
            (this.country = ''),
            (this.region = ''),
            (this.city = ''),
            (this.street = ''),
            (this.house = ''),
            (this.choosenDocument = ''),
            (this.series = ''),
            (this.number = ''),
            (this.issuedBy = ''),
            (this.dateOfIssue = '');
      },
      submitHandler() {
         if (this.$v.$invalid) {
            this.$v.$touch();
            return;
         }
         this.showModal = true;
         const formData = {
            lastname: this.lastname,
            name: this.name,
            fatherName: this.fatherName,
            birthday: this.birthday,
            tel: this.tel,
            sex: this.sex,
            client: this.client,
            doctor: this.doctor,
            notSendSMS: this.notSendSMS,
            addres: {
               index: this.index,
               country: this.country,
               region: this.region,
               city: this.city,
               street: this.street,
               house: this.house,
            },
            passport: {
               document: this.document,
               series: this.series,
               number: this.number,
               issuedBy: this.issuedBy,
               dateOfIssue: this.dateOfIssue,
            },
         };
         console.log(formData);
      },
   },
};
</script>

<style lang="sass">
form
   width: 60%
   padding: 20px
   background: #607381

.input-field
   display: flex
   flex-direction: column
   width: 48%
   padding-bottom: 10px
.form-control
   border: 1px solid #ced4da
   border-radius: 3px
   padding: 10px
   font-family: cursive
label
   padding-bottom: 5px
.error
   color: #ad1010
button
   width: 5em
   font-size: 1.4em
   background: #dee6f2
   border: none
   padding: 0.3em 0.6em
   border-radius: 10px 10px 10px 10px
   cursor: pointer
@media (max-width: 550px)
   .input-field
      width: 100%
   .sm-flex-column
      flex-direction: column
</style>
