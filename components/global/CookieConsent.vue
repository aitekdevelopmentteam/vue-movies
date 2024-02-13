<template>
  <div
    v-if="isOpen"
    class="alert alert--alt">
    <p>Our website utilizes cookies and various tracking technologies to enhance your browsing experience. By accessing and navigating through our site, you agree to our employment of cookies and similar tracking mechanisms.. <a target="_blank" href="https://jason.codes/cookie-policy" rel="noopener">Find out more</a>.</p>

    <div>
      <button
        class="alert__btn alert__btn--secondary button"
        type="button"
        aria-label="Close"
        @click="decline">
        Decline
      </button>

      <button
        class="alert__btn alert__btn--primary button"
        type="button"
        aria-label="Close"
        @click="accept">
        Accept
      </button>
    </div>
  </div>
</template>

<script>
import { get, set } from 'tiny-cookie';
import { supportsLocalStorage } from '~/mixins/Functions';

export default {
  data () {
    return {
      isOpen: false,
      storageName: 'cookieconsent',
    };
  },

  mounted () {
    if (!this.getVisited()) {
      this.isOpen = true;
    }
  },

  methods: {
    getVisited () {
      if (supportsLocalStorage()) {
        return localStorage.getItem(this.storageName);
      } else {
        return get(this.storageName);
      }
    },

    setAccepted () {
      if (supportsLocalStorage()) {
        localStorage.setItem(this.storageName, 'accepted');
      } else {
        set(this.storageName, 'accepted');
      }
    },

    setDeclined () {
      if (supportsLocalStorage()) {
        localStorage.setItem(this.storageName, 'declined');
      } else {
        set(this.storageName, 'declined');
      }
    },

    accept () {
      this.setAccepted();
      this.isOpen = false;
    },

    decline () {
      this.setDeclined();
      this.isOpen = false;
    },
  },
};
</script>
