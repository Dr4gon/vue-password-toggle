<script>
export default {
  name: 'PasswordToggle',
  data() {
    return {
      passwordFieldType: 'password',
      passwordRequirements: [],
      passwordValid: false
    }
  },
  methods: {
    togglePasswordVisibility() {
      this.passwordFieldType = this.passwordFieldType === 'password' ? 'text' : 'password'
    },
    update(newValue) {
      this.$emit('update:password', newValue)
      this.validate(newValue)
    },
    validate(password) {
      this.passwordRequirements = [] // always clear before adding new requirements

      // idea inspired by https://dev.to/thormeier/use-all-the-features-how-to-create-a-fancy-password-input-with-vue3-ggi
      if (this.minPasswordLength > 0) {
        this.passwordRequirements.push({
          name: 'At least ' + this.minPasswordLength + ' characters',
          predicate: password.length >= this.minPasswordLength
        })
      }

      if (this.minOneUpperLetter) {
        this.passwordRequirements.push({
          name: 'At least one uppercase letter',
          predicate: /[A-Z]/.test(password)
        })
      }

      if (this.minOneLowerLetter) {
        this.passwordRequirements.push({
          name: 'At least one lowercase letter',
          predicate: /[a-z]/.test(password)
        })
      }

      if (this.minOneNumber) {
        this.passwordRequirements.push({
          name: 'At least one number',
          predicate: /\d/.test(password)
        })
      }

      if (this.minOneSpecialCharacter) {
        this.passwordRequirements.push({
          name: 'At least one special character',
          predicate: /[!@#$%^&*()_+\-=\[\]{};':"\\|,.<>\/?]+/.test(password)
        })
      }

      this.passwordValid = this.passwordRequirements.every((requirement) => requirement.predicate)
      this.$emit('passwordValid', this.passwordValid)
    }
  },
  props: {
    password: {
      type: String,
      required: true
    },
    minPasswordLength: {
      type: Number,
      required: false
    },
    minOneUpperLetter: {
      type: Boolean,
      default: false,
      required: false
    },
    minOneLowerLetter: {
      type: Boolean,
      default: false,
      required: false
    },
    minOneNumber: {
      type: Boolean,
      default: false,
      required: false
    },
    minOneSpecialCharacter: {
      type: Boolean,
      default: false,
      required: false
    }
  },
  emits: ['update:password', 'passwordValid', 'keydownEnter'] // sending the emits to the parent component
}
</script>

<template>
  <div class="password-combo">
    <input
      class="password"
      :type="this.passwordFieldType"
      :value="this.password"
      @input="update($event.target.value)"
      @keydown.enter="this.$emit('keydownEnter', $event)"
      placeholder="Password"
      required
    />
    <button class="pw-toggle-btn" @click.prevent="togglePasswordVisibility()">
      <img
        v-if="this.passwordFieldType === 'password'"
        alt="Show password"
        src="@/assets/eye-regular.svg"
        width="38"
        height="38"
      />
      <img
        v-if="this.passwordFieldType === 'text'"
        alt="Hide password"
        src="@/assets/eye-slash-regular.svg"
        width="38"
        height="38"
      />
    </button>
  </div>
  <ul class="requirements">
    <li
      v-for="(requirement, key) in passwordRequirements"
      :key="key"
      :class="requirement.predicate ? 'is-success' : 'is-error'"
    >
      {{ requirement.name }}
    </li>
  </ul>
</template>

<style scoped>
.password-combo {
  display: flex;
  width: 100%;
}
.pw-toggle-btn {
  margin-bottom: 1rem;
}
.requirements {
  font-weight: bold;
}

.is-success {
  color: #96ca2d;
}

.is-error {
  color: #ba3637;
}
</style>
