<script>
export default {
  name: 'PasswordToggle',
  data() {
    return {
      passwordFieldType: 'password',
      passwordRequirements: []
    }
  },
  methods: {
    togglePasswordVisibility() {
      this.passwordFieldType = this.passwordFieldType === 'password' ? 'text' : 'password'
    },
    validate(password) {
      this.passwordRequirements = [
        {
          name: 'At least 8 characters',
          predicate: password.length >= 8
        },
        {
          name: 'At least one uppercase letter',
          predicate: /[A-Z]/.test(password)
        },
        {
          name: 'At least one lowercase letter',
          predicate: /[a-z]/.test(password)
        },
        {
          name: 'At least one number',
          predicate: /\d/.test(password)
        },
        {
          name: 'At least one special character',
          predicate: /[!@#$%^&*()_+\-=\[\]{};':"\\|,.<>\/?]+/.test(password)
        }
      ]
    }
  },
  props: {
    password: {
      type: String,
      required: true
    }
  },
  emits: ['update:password'] // needed to update the parent's password
}
</script>

<template>
  <div class="password-combo">
    <input
      class="password"
      :type="this.passwordFieldType"
      :value="this.password"
      @input="$emit('update:password', $event.target.value)"
      @change="validate(this.password)"
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
