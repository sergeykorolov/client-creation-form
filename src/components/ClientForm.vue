<template>
    <div>
        <div class="contrast-background"></div>
        <div class="form-wrapper">
            <form @submit.prevent="submitHandler">
                <h1>Создание клиента</h1>
                <div class="form-group">
                    <h2>Основная информация:</h2>
                    <div class="form-item">
                        <label for="lastname">Фамилия</label>
                        <span class="required-star">*</span>
                        <input id="lastname"
                               v-model.trim="lastname"
                               :class="{invalid: ($v.lastname.$dirty && !$v.lastname.required)}"
                        >
                        <small class="input-error" v-if="$v.lastname.$dirty && !$v.lastname.required">
                            Поле Фамилия не должно быть пустым
                        </small>
                    </div>
                    <div class="form-item">
                        <label for="firstname">Имя</label>
                        <span class="required-star">*</span>
                        <input id="firstname"
                               v-model.trim="firstname"
                               :class="{invalid: ($v.firstname.$dirty && !$v.firstname.required)}"
                        >
                        <small class="input-error" v-if="$v.firstname.$dirty && !$v.firstname.required">
                            Поле Имя не должно быть пустым
                        </small>
                    </div>
                    <div class="form-item">
                        <label for="patronymic">Отчество</label>
                        <input id="patronymic" v-model.trim="patronymic">
                    </div>
                    <div class="form-item">
                        <label for="birth-date">Дата рождения</label>
                        <span class="required-star">*</span>
                        <input
                                type="date"
                                id="birth-date"
                                v-model="birthDate"
                                :class="{invalid: ($v.birthDate.$dirty && !$v.birthDate.required)}"
                        >
                        <small class="input-error" v-if="$v.birthDate.$dirty && !$v.birthDate.required">
                            Поле Дата рождения не должно быть пустым
                        </small>
                    </div>
                    <div class="form-item">
                        <label for="phone-number">Номер телефона</label>
                        <span class="required-star">*</span>
                        <input
                                type="number"
                                id="phone-number"
                                v-model="phoneNumber"
                                :class="{invalid: ($v.phoneNumber.$dirty && !$v.phoneNumber.phoneValidator)}"
                        >
                        <small class="input-error" v-if="phoneNumber[0] !== '7'">
                            Номер телефона должен начинаться с цифры 7
                        </small>
                        <small class="input-error" v-else-if="$v.phoneNumber.$dirty && !$v.phoneNumber.phoneValidator">
                            Номер телефона должен состоять из 11 цифр
                        </small>
                    </div>
                    <div class="form-item">
                        <label for="gender">Пол</label>
                        <input id="gender" v-model="gender">
                    </div>
                    <div class="form-item" :class="{invalid: ($v.clientsGroup.$dirty && !$v.clientsGroup.selectedGroupValid)}">
                        <label>Группа клиентов</label>
                        <span class="required-star">*</span>
                        <CustomSelect
                                :array-data="clientsGroup"
                                :is-multiselect="true"
                                :set-value="setClientsGroup"
                        />
                        <small class="input-error" v-if="$v.clientsGroup.$dirty && !$v.clientsGroup.selectedGroupValid">
                            Нужно выбрать хотябы 1 пункт
                        </small>
                    </div>
                    <div class="form-item">
                        <label>Лечащий врач</label>
                        <CustomSelect
                                :array-data="doctors"
                                :is-multiselect="false"
                                :set-value="setDoctor"
                        />
                    </div>
                    <div class="form-item">
                        <label class="checkbox-label">
                            <input type="checkbox" id="notification" v-model="sendingSMS">
                            <span class="check-wrap"><i class="ok"></i></span>
                        </label>
                        <label class="checkbox-label" for="notification">Не отправлять СМС</label>
                    </div>
                </div>

                <div class="form-group">
                    <h2>Адрес:</h2>
                    <div class="form-item">
                        <label for="postcode">Индекс</label>
                        <input type="number" id="postcode" v-model.trim="postcode">
                    </div>
                    <div class="form-item">
                        <label for="country">Страна</label>
                        <input id="country" v-model.trim="country">
                    </div>
                    <div class="form-item">
                        <label for="region">Регион</label>
                        <input id="region" v-model.trim="region">
                    </div>
                    <div class="form-item">
                        <label for="city">Город</label>
                        <span class="required-star">*</span>
                        <input id="city" v-model.trim="city" :class="{invalid: ($v.city.$dirty && !$v.city.required)}">
                        <small class="input-error" v-if="$v.city.$dirty && !$v.city.required">
                            Поле город обязательно
                        </small>
                    </div>
                    <div class="form-item">
                        <label for="street">Улица</label>
                        <input id="street" v-model.trim="street">
                    </div>
                    <div class="form-item">
                        <label for="house">Дом</label>
                        <input id="house" v-model.trim="house">
                    </div>
                </div>

                <div class="form-group">
                    <h2>Документ:</h2>
                    <div class="form-item" :class="{invalid: ($v.documentTypes.$dirty && !$v.documentTypes.selectedDocumentValid)}">
                        <label>Тип документа</label>
                        <span class="required-star">*</span>
                        <CustomSelect
                                :array-data="documentTypes"
                                :is-multiselect="false"
                                :set-value="setDocumentType"
                        />
                        <small class="input-error" v-if="$v.documentTypes.$dirty && !$v.documentTypes.selectedDocumentValid">
                            Выберите тип документа
                        </small>
                    </div>
                    <div class="form-item">
                        <label for="seriesOfPassport">Серия</label>
                        <input type="number" id="seriesOfPassport" v-model.trim="seriesOfPassport">
                    </div>
                    <div class="form-item">
                        <label for="numberOfPassport">Номер</label>
                        <input type="number" id="numberOfPassport" v-model.trim="numberOfPassport">
                    </div>
                    <div class="form-item">
                        <label for="whoIssuedPassport">Кем выдан</label>
                        <input id="whoIssuedPassport" v-model.trim="whoIssuedPassport">
                    </div>
                    <div class="form-item">
                        <label for="date-of-issue">Дата выдачи</label>
                        <span class="required-star">*</span>
                        <input
                                type="date"
                                id="date-of-issue"
                                v-model="dateOfIssue"
                                :class="{invalid: ($v.dateOfIssue.$dirty && !$v.dateOfIssue.required)}"
                        >
                        <small class="input-error" v-if="$v.dateOfIssue.$dirty && !$v.dateOfIssue.required">
                            Необходимо заполнить дату выдачи документа
                        </small>
                    </div>
                </div>
                <div class="require-message">Поля отмеченные <span class="required-star">*</span> обязательны для заполнения</div>
                <div class="success-message" v-if="showSuccessMessage">Клиент успешно добавлен !</div>
                <button type="submit">Сохранить</button>
            </form>
        </div>
    </div>
</template>

<script>
    import CustomSelect from "./formItems/CustomSelect";
    import {required, numeric, minLength, maxLength} from 'vuelidate/lib/validators'

    export default {
        name: "ClientForm",
        components: {CustomSelect},
        data() {
            return {
                lastname: '',
                firstname: '',
                patronymic: '',
                birthDate: '',
                phoneNumber: '7',
                gender: '',
                selectedGroup: [],
                selectedDoctor: '',
                sendingSMS: false,
                clientsGroup: [
                    {
                        label: 'VIP',     // label выводится пользователю
                        value: 'vip'      // value уходит на сервер
                    },
                    {
                        label: 'Проблемные',
                        value: 'problematic'
                    },
                    {
                        label: 'ОМС',
                        value: 'omc'
                    },
                ],
                doctors: [
                    {
                        label: 'Иванов',
                        value: 'ivanov'
                    },
                    {
                        label: 'Захаров',
                        value: 'zaharov'
                    },
                    {
                        label: 'Чернышева',
                        value: 'chernysheva'
                    },
                ],
                postcode: '',
                country: '',
                region: '',
                city: '',
                street: '',
                house: '',
                selectedDocumentType: '',
                documentTypes: [
                    {
                        label: 'Паспорт',
                        value: 'passport'
                    },
                    {
                        label: 'Свидетельство о рождении',
                        value: 'birth-certificate'
                    },
                    {
                        label: 'Вод. удостоверение',
                        value: 'drivers-license'
                    },
                ],
                seriesOfPassport: '',
                numberOfPassport: '',
                whoIssuedPassport: '',
                dateOfIssue: '',
                showSuccessMessage: false
            }
        },
        validations: {
            lastname: {required},
            firstname: {required},
            birthDate: {required},
            phoneNumber: {
                phoneValidator(value) {
                    let phone_regex = /^7\d{3}\d{3}\d{2}\d{2}$/
                    return phone_regex.test(value)
                }
            },
            clientsGroup: {
                selectedGroupValid() {
                    return this.selectedGroup.length > 0
                }
            },
            city: {required},
            documentTypes: {
                selectedDocumentValid() {
                    return this.selectedDocumentType !== ''
                }
            },
            dateOfIssue: {required}
        },
        methods: {
            submitHandler() {
                if (this.$v.$invalid) {
                    this.$v.$touch()  // активизирует валидацию
                    return
                }
                this.showSuccessMessage = true
                setTimeout(() => this.showSuccessMessage = false, 4000)
            },
            setDoctor(doctor) {
                this.selectedDoctor = doctor
            },
            setDocumentType(documentType) {
                this.selectedDocumentType = documentType
            },
            setClientsGroup(clientsGroup) {
                this.selectedGroup = clientsGroup.map(clientGroup => clientGroup.value)
            }
        }
    }
</script>

<style lang="scss">
    /*variables*/
    $mainBgColor: #eee;
    $mainTextColor: #333;
    $inputBorderColor: #c3c3c3;
    $invalidColor: #c13434;
    $darkColor: #000;
    $contrastColor: #67dfd5;
    $darkContrastColor: #12736b;

    body {
        margin: 0;
        min-width: 300px;
        color: $mainTextColor;
        font-family: 'Roboto', sans-serif;
    }

    .contrast-background {
        position: fixed;
        width: 800px;
        height: 800px;
        background-color: $contrastColor;
        left: 50%;
        transform: rotate(45deg);
        top: -400px;
        margin-left: -640px;
    }

    .form-wrapper {
        /*<!--background: $mainBgColor;-->*/
        position: relative;
        padding-bottom: 50px;
        .require-message {
            margin-bottom: 15px;
            font-size: 14px;
            .required-star {
                color: $invalidColor;
            }
        }
        .success-message {
            color: $darkContrastColor;
            margin-bottom: 15px;
        }
        button {
            padding: 10px;
            text-transform: uppercase;
            border: none;
            border-radius: 5px;
            font-weight: 700;
            background-color: $contrastColor;
            color: #fff;
            box-shadow: 0 0 2px 1px #fff;
            cursor: pointer;
            outline: none;
            transition: background-color 0.1s;
            &:hover {
                background-color: $darkContrastColor;
            }
        }
    }

    .form-group {
        margin-bottom: 15px;
        border-radius: 5px;
        background: #fff;
        padding: 15px 25px 15px 20px;
        box-shadow: 2px 2px 5px 0 #0000001f;
        h2 {
            margin-top: 0;
            font-size: 20px;
            font-weight: 700;
        }
        label {
            display: block;
            opacity: 0.7;
        }
    }

    form {
        width: 100%;
        max-width: 500px;
        margin: 0 auto;

        h1 {
            margin: 0;
            padding: 50px 0 30px 0;
        }

        input {
            padding: 8px 10px;
            font-size: 16px;
            border: 1px solid $inputBorderColor;
            border-radius: 4px;
            width: 100%;
            box-sizing: border-box;

            /* Chrome, Safari, Edge, Opera */
            &::-webkit-outer-spin-button,
            &::-webkit-inner-spin-button {
                -webkit-appearance: none;
                margin: 0;
            }

            /* Firefox */
            &[type=number] {
                -moz-appearance: textfield;
            }
            &.invalid {
                border-color: $invalidColor;
            }

            &[type=checkbox] {
               display: none;
                &:checked ~ .check-wrap .ok:after{
                    display: block;
                }
            }
        }

        .form-item.invalid .selected {
            border-color: $invalidColor;
        }

        .form-item {
            margin-bottom: 10px;
            .input-error {
                color: $invalidColor;
            }
            .required-star{
                float: right;
                margin-right: -11px;
                color: $invalidColor;
            }
        }

        .checkbox-label{
            display: inline-block;
            vertical-align: middle;
            opacity: 1;
            cursor: pointer;
        }

        .check-wrap {
            position: relative;
            display: inline-block;
            border: 1px solid $inputBorderColor;
            width: 22px;
            height: 22px;
            background: #fff;
            margin-right: 10px;
            border-radius: 4px;
        }
        .ok:after {
            content: "✔";
            display: none;
            position: absolute;
            font-size: 18px;
            margin: -2px 0 0 2px;
            color: $darkColor;
            opacity: 0.5;
            transform: rotate(-7deg);
        }
    }
</style>
