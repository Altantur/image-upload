<template>
  <v-app>
    <nuxt />
    <v-footer
      absolute
      app
      color="transparent"
    >
      <v-col
        class="text-center greysh-text"
        cols="12"
      >
        created by
        <strong><a class="text-decoration-none greysh-text" href="https://altantur.me" target="_blank"> altantur</a></strong> -
        <a class="text-decoration-none greysh-text" href="https://devchallenges.io/" target="_blank"> devChallenges.io</a>
      </v-col>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
    }
  }
}
</script>
