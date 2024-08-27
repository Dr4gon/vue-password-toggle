<script>
export default {
  name: 'PasswordToggle',
  data() {
    return {
      passwordFieldType: 'password'
    }
  },
  methods: {
    togglePasswordVisibility() {
      this.passwordFieldType = this.passwordFieldType === 'password' ? 'text' : 'password'
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
</template>

<style scoped>
.password-combo {
  display: flex;
  width: 100%;
}
.pw-toggle-btn {
  margin-bottom: var(--pico-spacing); /** Adjust to the input field pico style */
}
</style>
