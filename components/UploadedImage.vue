<template>
  <div>
    <v-card
      class="d-flex flex-column justify-space-around align-center container rounded-lg"
      width="402"
    >
      <v-icon
        large
        class="mt-1"
        color="green darken-2"
      >
        mdi-checkbox-marked-circle
      </v-icon>
      <p class="title mb-5">
        Uploaded Successfully!
      </p>
      <v-img
        class="rounded-lg"
        width="338px"
        height="224.4px"
        :src="url"
      />
      <v-card-actions>
        <div class="link">
          <div class="url">
            {{ url }}
          </div>
          <v-btn
            class="copy-link-button rounded-lg"
            color="primary"
            depressed
            @click="copyToClipboard()"
          >
            Copy Link
          </v-btn>
        </div>
      </v-card-actions>
    </v-card>
    <v-snackbar
      v-model="snackbar"
      light
      outlined
      bottom
      class="text-center"
    >
      <v-row justify="center" align="center" @click="snackbar = false">
        Copied to clipboard
        <v-icon
          color="green darken-2"
        >
          mdi-clipboard-text
        </v-icon>
      </v-row>
    </v-snackbar>
  </div>
</template>
<script>
export default {
  props: {
    url: {
      default: () => '',
      type: String
    }
  },
  data: () => ({
    snackbar: false
  }),
  methods: {
    copyToClipboard () {
      const text = this.url
      if (window.clipboardData && window.clipboardData.setData) {
        return window.clipboardData.setData('Text', text)
      } else if (document.queryCommandSupported && document.queryCommandSupported('copy')) {
        const textarea = document.createElement('textarea')
        textarea.textContent = text
        textarea.style.position = 'fixed'
        document.body.appendChild(textarea)
        textarea.select()
        this.snackbar = true
        try { return document.execCommand('copy') } catch (ex) { return ex } finally {
          document.body.removeChild(textarea)
        }
      }
    }
  }
}
</script>
