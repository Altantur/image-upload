<template>
  <div>
    <v-card
      v-if="!uploading && !uploaded"
      class="d-flex flex-column justify-space-around align-center container rounded-lg"
      width="402"
    >
      <p class="title">
        Upload your image
      </p>
      <p class="file-type" :class="{warn: warn}">
        File should be jpeg, png,...
      </p>
      <div class="upload-zone" :class="{dragover: active}" @dragover="dragover" @dragleave="dragleave" @drop="drop">
        <div class="upload-back-image" />
        <input
          ref="file"
          type="file"
          name="file"
          class="file"
          accept=".jpg,.jpeg,.png"
          @change="onChange"
        >
        <div class="drag-drop">
          Drag & Drop your image here
        </div>
      </div>

      <div class="or-text">
        Or
      </div>

      <v-card-actions>
        <v-btn
          class="upload-button rounded-lg"
          color="primary"
          depressed
          @click="$refs.file.click()"
        >
          Choose a file
        </v-btn>
      </v-card-actions>
    </v-card>
    <Loading v-if="uploading" />
    <UploadedImage v-if="uploaded" :url="url" />
  </div>
</template>
<script>
export default {
  delimiters: ['${', '}'], // Avoid Twig conflicts
  data: () => ({
    uploading: false,
    uploaded: false,
    active: false,
    warn: false,
    url: '',
    file: null
  }),
  methods: {
    onChange () {
      if (this.$refs.file.files.length) {
        this.file = this.$refs.file.files[0]
        this.upload()
      }
    },
    dragover (event) {
      event.preventDefault()
      this.active = true
    },
    dragleave (event) {
      this.active = false
    },
    drop (event) {
      event.preventDefault()
      this.active = false
      if (event.dataTransfer.files.length) {
        this.file = event.dataTransfer.files[0]
        this.warn = event.dataTransfer.files[0].type.split('/')[0] !== 'image'
        setTimeout(() => { this.warn = false }, 2000)
      }
      if (!this.warn) {
        this.upload()
      }
    },
    dropable () {
      const div = document.createElement('div')
      return (('draggable' in div) || ('ondragstart' in div && 'ondrop' in div)) && 'FormData' in window && 'FileReader' in window
    },
    uid () {
      let firstPart = (Math.random() * 46656) | 0
      let secondPart = (Math.random() * 46656) | 0
      firstPart = ('000' + firstPart.toString(36)).slice(-3)
      secondPart = ('000' + secondPart.toString(36)).slice(-3)
      return firstPart + secondPart
    },
    upload () {
      this.uploading = true
      const ref = this.$fire.storage.ref('/image-upload/' + this.uid())
      ref.put(this.file).then((snapshot) => {
        ref.getDownloadURL().then((url) => {
          this.url = url
          this.uploading = false
          this.uploaded = true
        }).catch(() => {
          this.uploading = false
        })
      })
    }
  }
}
</script>
