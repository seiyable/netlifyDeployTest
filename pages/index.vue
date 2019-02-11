<template>
  <section class="container">
    <div>
      <logo />
      <h1 class="title">
        netlifyDeployTest
      </h1>
      <h2 class="subtitle">
        My super Nuxt.js project
      </h2>
      <div class="links">
        <a
          href="https://nuxtjs.org/"
          target="_blank"
          class="button--green"
        >
          Documentation
        </a>
        <a
          href="https://github.com/nuxt/nuxt.js"
          target="_blank"
          class="button--grey"
        >
          GitHub
        </a>
      </div>
      <ul class="env-var-list">
        <li>NODE_ENV: {{ nodeEnv }}</li>
        <li>NODE_VERSION: {{ nodeVersion }}</li>
        <li>MY_VALUE: {{ myValue }}</li>
      </ul>
      <ul class="contentful">
        <li
          v-for="(entry, index) in entries"
          :key="index"
        >
          {{ entry }}
        </li>
      </ul>
    </div>
  </section>
</template>

<script>
import Logo from '~/components/Logo.vue'
import { createClient } from 'contentful'
const client = createClient({
  space: process.env.CONTENTFUL_SPACE_ID,
  accessToken:
    process.env.CONTENTFUL_CDAPI_ACCESS_TOKEN
})

export default {
  components: {
    Logo
  },
  data() {
    return {
      nodeEnv: '',
      nodeVersion: '',
      myValue: '',
      entries: [],
      error: ''
    }
  },
  created() {
    this.nodeEnv = process.env.NODE_ENV
    this.nodeVersion = process.env.NODE_VERSION
    this.myValue = process.env.MY_VALUE

    client.getEntries()
      .then((response) => {
        this.entries = response.items
      })
      .catch((error) => {
        this.error = error
      })
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}

.env-var-list {
  margin: 20px auto;
  width: 500px;
  text-align: left;
}
</style>
