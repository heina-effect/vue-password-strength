<template>
  <div class="main-container">
    <div class="header-section">
      <h1>PASSWORD STRENGTH</h1>
    </div>
    <div class="container-section">
      <div class="input_container">
        <ul>
          <li v-bind:class="{ is_valid : contains_eight_characters }">
            8 Characters
          </li>
          <li v-bind:class="{ is_valid: contains_number }">Contains Number</li>
          <li v-bind:class="{ is_valid: contains_uppercase }">Contains Uppercase</li>
          <li v-bind:class="{ is_valid: contains_special_character }">Contains Special Character</li>
        </ul>
      </div>
      <div
        class="checkmark_container"
        v-bind:class="{ show_checkmark: valid_password }"
      >
        <svg width="50%" height="50%" viewBox="0 0 140 100">
          <path
            class="checkmark"
            v-bind:class="{ checked: valid_password }"
            d="M10,50 l25,40 l95,-70"
          />
        </svg>
      </div>
      <div class="input_box">
        <v-text-field
          v-model="password"
          :append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
          :type="show ? 'text' : 'password'" 
          @input="checkPassword" 
          label="Password"
          hint="At least 8 characters"
          @click:append="show =! show"
          counter
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "passwordStrength",
  data: () => ({
    password: null,
    password_length: 0,
    contains_eight_characters: false,
    contains_number: false,
    contains_uppercase: false,
    contains_special_character: false,
    valid_password: false,
    show : false
  }),
  methods: {
    checkPassword() {
      this.password_length = this.password.length;
      const format = /[!@#$%^&*()_+\-=,.?<>/;':"{}|\\[\]]/; // eslint-disable-line no-unused-vars

      this.password_length > 8
        ? (this.contains_eight_characters = true)
        : (this.contains_eight_characters = false);

      this.contains_number = /\d/.test(this.password);
      this.contains_uppercase = /[A-Z]/.test(this.password);
      this.contains_special_character = format.test(this.password);

      if (this.contains_number === true 
      && this.contains_eight_characters === true
      && this.contains_uppercase === true
      && this.contains_special_character === true
      ) {
        this.valid_password = true;
      } else {
        this.valid_password = false;
      }
    },
  },
};
</script>

<style lang="scss" >
html {
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: inherit;
}

html,
body {
  height: 100%;
  width: 100%;
  margin: 0;
  padding: 0;
}

body {
  font-family: sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
}

.v-application--wrap {
  min-height: 0vh !important;
}

h1 {
  text-align: center;
}

.container-section {
  box-shadow: 0 5px 20px rgba(0, 0, 0, 0.19), 0 6px 6px rgba(0, 0, 0, 0.23);
  border-radius: 10px;
  padding: 1rem;

  ul {
    padding-left: 20px;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }

  li {
    margin-bottom: 8px;
    color: #525f7f;
    position: relative;
  }

  li:before {
    content: "";
    width: 0%;
    height: 2px;
    background: #2ecc71;
    position: absolute;
    left: 0;
    top: 50%;
    display: block;
    transition: all 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  }

  /* Password Input --------- */

  // input[type="password"] {
  //   line-height: 1.5;
  //   font-weight: 300;
  //   width: 100%;
  //   height: calc(2.75rem + 2px);
  //   padding: 0.625rem 0.75rem;
  //   // border: 1px solid #cad1d7;
  //   border-radius: 0.25rem;
  //   transition: border-color 0.4s ease;
  //   outline: 0;
  // }

  // input[type="password"]:focus {
  //   border-color: rgba(50, 151, 211, 0.45);
  // }

  /* Checkmark & Strikethrough --------- */
  .is_valid {
    color: rgba(136, 152, 170, 0.8);
  }
  .is_valid:before {
    width: 100%;
  }

  .checkmark_container {
    border-radius: 50%;
    position: absolute;
    top: 25px;
    right: -25px;
    background: #2ecc71;
    width: 50px;
    height: 50px;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: opacity 0.4s ease;
    visibility: hidden;
    opacity: 1;
  }

  .show_checkmark {
    visibility: visible;
    opacity: 1;
  }

  .checkmark {
    width: 100%;
    height: 100%;
    fill: none;
    stroke: white;
    stroke-width: 15;
    stroke-linecap: round;
    stroke-dasharray: 180;
    stroke-dashoffset: 180;
  }

  .checked {
    animation: draw 0.5s ease forwards;
  }

  @keyframes draw {
    to {
      stroke-dashoffset: 0;
    }
  }
}
</style>
