<template>
  <div class="container">
    <h1 class="title">Register</h1>
    <b-form @submit.prevent="onRegister" @reset.prevent="onReset">
      <b-form-group
          id="input-group-username"
          label-cols-sm="3"
          label="Username:"
          label-for="username"
      >
        <b-form-input
            id="username"
            v-model="$v.form.username.$model"
            type="text"
            :state="validateState('username')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.username.required">
          Username is required
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-else-if="!$v.form.username.length">
          Username length should be between 3-8 characters long
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-if="!$v.form.username.alpha">
          Please insert Only letters
        </b-form-invalid-feedback>

      </b-form-group>

      <b-form-group
          id="input-group-first-name"
          label-cols-sm="3"
          label="First Name:"
          label-for="first-name"
      >
        <b-form-input
            id="first-name"
            v-model="$v.form.firstName.$model"
            type="text"
            :state="validateState('firstName')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.firstName.required">
          First name is required
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-if="!$v.form.firstName.alpha">
          Please insert Only letters
        </b-form-invalid-feedback>

      </b-form-group>

      <b-form-group
          id="input-group-last-name"
          label-cols-sm="3"
          label="Last Name:"
          label-for="last-name"
      >
        <b-form-input
            id="last-name"
            v-model="$v.form.lastName.$model"
            type="text"
            :state="validateState('lastName')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.lastName.required">
          Last name is required
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-if="!$v.form.lastName.alpha">
          Please insert Only letters
        </b-form-invalid-feedback>

      </b-form-group>

      <b-form-group
          id="input-group-country"
          label-cols-sm="3"
          label="Country:"
          label-for="country"
      >
        <b-form-select
            id="country"
            v-model="$v.form.country.$model"
            :options="countries"
            :state="validateState('country')"
        ></b-form-select>
        <b-form-invalid-feedback>
          Country is required
        </b-form-invalid-feedback>
      </b-form-group>

      <b-form-group
          id="input-group-Password"
          label-cols-sm="3"
          label="Password:"
          label-for="password"
      >
        <b-form-input
            id="password"
            type="password"
            v-model="$v.form.password.$model"
            :state="validateState('password')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.password.required">
          Password is required
        </b-form-invalid-feedback>
        <b-form-invalid-feedback
            v-if="$v.form.password.required && !$v.form.password.length && !$v.form.password.oneNumber && !$v.form.password.oneChar"
        >Have length between 5-10 characters long, At least one number is required, At least one special char is required
        </b-form-invalid-feedback>
        <b-form-text v-else-if="$v.form.password.$error" text-variant="info">
          Your password should be <strong>strong</strong>. <br />
        </b-form-text>

      </b-form-group>

      <b-form-group
          id="input-group-confirmedPassword"
          label-cols-sm="3"
          label="Confirm Password:"
          label-for="confirmedPassword"
      >
        <b-form-input
            id="confirmedPassword"
            type="password"
            v-model="$v.form.confirmedPassword.$model"
            :state="validateState('confirmedPassword')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.confirmedPassword.required">
          Password confirmation is required
        </b-form-invalid-feedback>
        <b-form-invalid-feedback
            v-else-if="!$v.form.confirmedPassword.sameAsPassword"
        >
          The confirmed password is not equal to the original password
        </b-form-invalid-feedback>
      </b-form-group>

      <b-form-group
          id="input-group-email"
          label-cols-sm="3"
          label="Email:"
          label-for="email"
      >
        <b-form-input
            id="email"
            v-model="$v.form.email.$model"
            type="text"
            :state="validateState('email')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.email.required">
          Email is required
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-if="!$v.form.email.email">
          Please insert valid email
        </b-form-invalid-feedback>
      </b-form-group>

      <b-form-group
          id="input-group-profile"
          label-cols-sm="3"
          label="Profile Picture:"
          label-for="profile"
      >
        <b-form-input
            id="profile"
            v-model="$v.form.profile.$model"
            type="text"
            :state="validateState('profile')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.profile.required">
          Profile picture is required
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-if="!$v.form.profile.url">
          Please insert valid url
        </b-form-invalid-feedback>
      </b-form-group>

      <b-button type="reset" variant="danger">Reset</b-button>
      <b-button
          type="submit"
          variant="primary"
          style="width:250px;"
          class="ml-5 w-75"
      >Register</b-button
      >
      <div class="mt-2">
        You have an account already?
        <router-link to="login"> Log in here</router-link>
      </div>
    </b-form>
    <b-alert
        class="mt-2"
        v-if="form.submitError"
        variant="warning"
        dismissible
        show
    >
      Register failed: {{ form.submitError }}
    </b-alert>
  </div>
</template>

<script>
    import countries from "../assets/countries";
    import {
        required,
        minLength,
        maxLength,
        alpha,
        sameAs,
        url,
        email
    } from "vuelidate/lib/validators";
    import axios from 'axios';
    export default {
        name: "Register",
        data() {
            return {
                form: {
                    username: "",
                    firstName: "",
                    lastName: "",
                    country: null,
                    password: "",
                    confirmedPassword: "",
                    email: "",
                    profile: "",
                    submitError: undefined
                },
                countries: [{ value: null, text: "", disabled: true }],
                errors: [],
                validated: false
            };
        },
        validations: {
            form: {
                username: {
                    required,
                    length: (u) => minLength(3)(u) && maxLength(8)(u),
                    alpha
                },
                country: {
                    required
                },
                firstName: {
                    required,
                    alpha
                },
                email: {
                    required,
                    email
                },
                lastName: {
                    required,
                    alpha
                },
                profile: {
                    url,
                    required
                },
                password: {
                    required,
                    length: (p) => minLength(5)(p) && maxLength(10)(p),
                    oneChar: (u) => !u.match(/^[A-Z,a-z,0-9]*$/),
                    oneNumber: (u) => !(u.search(/\d/) === -1)
                },
                confirmedPassword: {
                    required,
                    sameAsPassword: sameAs("password")
                }
            }
        },
        mounted() {
            console.log("mounted");
            this.countries.push(...countries);
            // console.log($v);
        },
        methods: {
            validateState(param) {
                const { $dirty, $error } = this.$v.form[param];
                return $dirty ? !$error : null;
            },
            async Register() {
                try {
                    this.form.submitError = undefined;
                    const response = await axios.post(
                            "http://localhost/user/Register",
                        {
                            username: this.form.username,
                            password: this.form.password,
                            firstName: this.form.firstName,
                            lastName: this.form.lastName,
                            email: this.form.email,
                            profilePic: this.form.profile,
                            country: this.form.country
                        }
                    );
                    this.$router.push("/login");
                    console.log("response:")
                    console.log(response);
                } catch (err) {
                    console.log(err);
                    console.log(err.response);
                    this.form.submitError = err.response.data.message;
                    console.log(this.form.submitError)
                }
            },
            onRegister() {
                console.log("register method called");
                this.$v.form.$touch();
                if (this.$v.form.$anyError) {
                    return;
                }
                console.log("register method go");
                this.Register();
            },
            onReset() {
                this.form = {
                    username: "",
                    firstName: "",
                    lastName: "",
                    country: null,
                    password: "",
                    profile: "",
                    confirmedPassword: "",
                    email: "",
                    submitError: undefined
                };
                this.$nextTick(() => {
                    this.$v.$reset();
                });
            }
        }
    };
</script>
<style lang="scss" scoped>
  .container {
    max-width: 500px;
  }
</style>