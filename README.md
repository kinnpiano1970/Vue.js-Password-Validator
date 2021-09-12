# Vue.js-Password-Validator

A Vue.js password validator. Evaluates each character as it's typed into the password field. When the password meets the required criteria, the red X changes to a green check mark. When password meets all criteria the register button is enabled.

The password must meet the follwing criteria.

- Must have at least 8 characters
- Must match password repeat
- Must have at one UPPERCASE
- Must have at least one number
- Must contain NO SPACES
- Must have at least one SPECIAL CHARACTER

````javascript

const app = new Vue({
  el: "#app",
  data: {
    validated: true,
    password: "",
    passRepeat: "",
    inputType: "password",
    isPasswordValid: false,
    filters: {
      isNumber: new RegExp(/[0-9]/),
      isSpace: new RegExp(/\s/),
      isUpper: new RegExp(/[A-Z]/),
      isSpecChar: new RegExp(/[!@#$%^&*()]/),
    },
    validators: {
      hasEight: false, //done
      hasMatch: false, //done
      hasUpper: false, //done
      hasNumber: false, //done
      hasNoSpace: false, //done
      hasSpecChar: false, //done
    },
  },
  methods: {
    keyPress() {
      // Make expressions a bit more readable
      let filter = this.filters,
        validate = this.validators,
        password = this.password,
        passwordRepeat = this.passwordRepeat;

      // Check if 8 or more characters
      validate.hasEight = password.length >= 8 ? true : false;

      // Checks if passwords match
      validate.hasMatch = this.passRepeat === password ? true : false;

      // Check if has a NUMBER
      validate.hasNumber = filter.isNumber.test(password) ? true : false;

      // Check if has an UPPERCASE
      validate.hasUpper = filter.isUpper.test(password) ? true : false;

      // Check for spaces
      validate.hasNoSpace = !filter.isSpace.test(password) ? true : false;

      // Check for a SPECIAL CHARACTER
      validate.hasSpecChar = filter.isSpecChar.test(password) ? true : false;

      this.validate();
      this.handleSpecialCase();
    },
    isChecked() {
      // Make password fields visible
      this.inputType = event.target.checked ? "text" : "password";
    },
    validate() {
      let tempArr = [];

      // Create TEMPORARY ARRAY from the VALIDATORS OBJECT
      for (const validator in this.validators) {
        tempArr.push(this.validators[validator]);
      }
      // Checks all validators.
      // If all TRUE register button is enabled
      this.isPasswordValid = tempArr.every((item) => {
        return item === true;
      });
    },
    /* Resets hasMatch & hasNoSpace validator if user 
        backspaces password fields all the way back */
    handleSpecialCase() {
      let passLen = this.password.length;
      let passRptLen = this.passRepeat.length;

      if (passLen === 0 && passRptLen === 0) {
        this.validators.hasMatch = false;
        this.validators.hasNoSpace = false;
      }
    },
  },
  computed: {},
  watch: {
    // Input from Password Field
    password(value) {
      //console.log('Pass: '+value.length);
      //console.log('PassRpt: '+this.password.length);
    },
    passRepeat(value) {
      // console.log('Pass: '+this.password.length);
      // console.log('PassRpt: '+value.length);
    },
  },
});



````
