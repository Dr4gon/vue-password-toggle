<script>
import PasswordToggle from './lib/PasswordToggle.vue'

export default {
  name: 'App',
  components: {
    PasswordToggle
  },
  data() {
    return {
      password: '',
      passwordValid: false
    }
  },
  methods: {
    doLogin(event) {
      event.preventDefault()

      if (this.passwordValid) {
        console.log('doLogin', event)
      } else {
        console.log('Password is not valid')
      }
    }
  }
}
</script>

<template>
  <header>
    <!-- Replace this stylesheet with bootstrap or whatever you like to adjust styling -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@picocss/pico@2/css/pico.min.css" />

    <div class="wrapper">
      <!-- https://vueschool.io/articles/vuejs-tutorials/techniques-for-sharing-data-between-vue-js-components/ -->
      <PasswordToggle
        v-model:password="password"
        @passwordValid="
          (payload) => {
            console.log('passwordValid', payload)
            this.passwordValid = payload
          }
        "
        @keydownEnter="this.doLogin($event)"
        :minPasswordLength="9"
        :minOneLowerLetter="true"
        :minOneUpperLetter="true"
        :minOneNumber="true"
        :minOneSpecialCharacter="true"
      />
    </div>
  </header>
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
  width: 77vw;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
  justify-content: center;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  font-size: 22px;
  display: inline-block;
  padding: 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
    width: 23vw;
  }

  .logo {
    margin: 2rem 0 2rem 0;
    width: 22vw;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    justify-content: center;
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem;
    margin-top: 1rem;
  }
}
</style>
