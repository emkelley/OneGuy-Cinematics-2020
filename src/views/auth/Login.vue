<template>
  <div id="login">
    <div id="app">
      <div class="login-card has-text-centered">
        <div class="card-title va">
          <center>
            <img src="~@/assets/img/logo-horizontal-white.svg" width="250" />
          </center>
        </div>
        <br />
        <p class="help status">Log in with a provider below:</p>
        <br />
        <a @click="signIn('google')" class="button is-inverted is-medium">
          <i class="fab fa-google" style="margin-right: 0.75rem"></i> Log in
          with Google
        </a>
        <br /><br />
      </div>
    </div>
  </div>
</template>

<script>
import firebase from '@firebase/app'
import '@firebase/auth'

export default {
  layout: 'auth',
  methods: {
    signIn(authProvider) {
      let provider
      if (authProvider === 'google') {
        provider = new firebase.auth.GoogleAuthProvider()
      }
      firebase
        .auth()
        .setPersistence(firebase.auth.Auth.Persistence.SESSION)
        .then(() => {
          firebase
            .auth()
            .signInWithPopup(provider)
            .then(user => {
              const displayName = user.user.displayName
              if (user.user.email === 'e.kelley94@gmail.com')
                this.$store.commit('setAdmin', true)
              this.$store.commit('setUserDisplayName', displayName)
              this.$store.commit(
                'setUserProfile',
                user.additionalUserInfo.profile
              )
              this.$store.commit('isAuthed', true)
              this.$router.push({
                path: '/cinematics/overwatch'
              })
            })
        })
    }
  }
}
</script>

<style lang="scss" scoped>
$grey-darker: hsl(0, 0%, 21%);
$grey-dark: hsl(0, 0%, 29%);
$grey: hsl(0, 0%, 48%);
$grey-light: hsl(0, 0%, 71%);
$grey-lighter: hsl(0, 0%, 86%);
.status {
  padding-left: 2rem;
  padding-right: 2rem;
}
#login {
  margin-top: -52px;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background: linear-gradient(
      rgba(255, 255, 255, 0.701) 0%,
      rgba(255, 255, 255, 0.612) 100%
    ),
    url('../../assets/hero-fallback.png');
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;

  backdrop-filter: saturate(200%) blur(10px) !important;

  .login-card {
    background: #fff;
    width: 24rem;
    box-shadow: $shadow-large;
    border-radius: 10px;

    .card-title {
      background-color: darken($primary, 5%);
      border-top-right-radius: 10px;
      border-top-left-radius: 10px;
      padding: 2rem;

      h1 {
        color: #fff;
        text-align: center;
        font-size: 1.2rem;
      }
    }

    .content {
      padding: 3rem 2.5rem 2rem;
    }

    #email,
    #password {
      display: block;
      width: 100%;
      font-size: 1rem;
      margin-bottom: 1.75rem;
      padding: 0.25rem 0;
      border: none;
      border-bottom: 1px solid $grey-lighter;
      transition: all 0.5s;

      &:hover {
        border-color: $grey;
      }

      &:active,
      &:focus {
        border-color: $primary;
      }
    }

    .checkbox {
      color: $grey-light;
      font-size: 0.8rem;

      span {
        margin-left: 0.5rem;
      }
    }

    .options {
      color: $grey-light;
      margin-bottom: 1.5rem;
    }

    button {
      cursor: pointer;
      font-size: 1.2rem;
      color: $primary;
      border-radius: 4rem;
      display: block;
      width: 100%;
      background: transparent;
      border: 2px solid $primary;
      padding: 0.9rem 0 1.1rem;
      transition: color 0.5s, border-color 0.5s;

      &:hover,
      &:focus {
        color: darken($primary, 10%);
        border-color: darken($primary, 10%);
      }

      &:active {
        transform: translateY(1px);
      }
    }
  }
}
#app {
  backdrop-filter: saturate(200%) blur(10px) !important;
}
label {
  cursor: pointer;
}

.regular-checkbox {
  display: none;
}

.regular-checkbox + label {
  background-color: #fafafa;
  border: 1px solid $grey-lighter;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
  padding: 7px;
  border-radius: 3px;
  display: inline-block;
  position: relative;
}

.regular-checkbox:checked + label {
  background-color: #e9ecee;
}

.regular-checkbox:checked + label:after {
  content: '\2714';
  font-size: 11px;
  position: absolute;
  top: 0;
  left: 3px;
  color: $grey-light;
}

input:focus,
select:focus,
textarea:focus,
button:focus {
  outline: none;
}

.login-btn {
  width: 100%;
  height: 3rem;
  a {
    font-size: 1.5rem;
  }
}
</style>
