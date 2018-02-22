<template>
  <div class="uploader">
    <div class="uploader__wrapper">
      <div class="uploader__input-wrapper">
        <template v-if="imageURL.length > 0">
          <button @click="removeImage()"
          class="uploader__input-remove" :class="{ 'is-persistent': preview === false }">
            <svg class="svg-icon" viewBox="0 0 20 20">
							<path d="M17.114,3.923h-4.589V2.427c0-0.252-0.207-0.459-0.46-0.459H7.935c-0.252,0-0.459,0.207-0.459,0.459v1.496h-4.59c-0.252,0-0.459,0.205-0.459,0.459c0,0.252,0.207,0.459,0.459,0.459h1.51v12.732c0,0.252,0.207,0.459,0.459,0.459h10.29c0.254,0,0.459-0.207,0.459-0.459V4.841h1.511c0.252,0,0.459-0.207,0.459-0.459C17.573,4.127,17.366,3.923,17.114,3.923M8.394,2.886h3.214v0.918H8.394V2.886z M14.686,17.114H5.314V4.841h9.372V17.114z M12.525,7.306v7.344c0,0.252-0.207,0.459-0.46,0.459s-0.458-0.207-0.458-0.459V7.306c0-0.254,0.205-0.459,0.458-0.459S12.525,7.051,12.525,7.306M8.394,7.306v7.344c0,0.252-0.207,0.459-0.459,0.459s-0.459-0.207-0.459-0.459V7.306c0-0.254,0.207-0.459,0.459-0.459S8.394,7.051,8.394,7.306"></path>
						</svg>
          </button>

          <p v-if="preview === false" class="uploader__file-name">{{ imageURL }}</p>

          <div v-if="preview" class="uploader__input-preview" :style="{ backgroundImage: `url(${imageURL})` }"></div>
        </template>

        <template v-else>
          <input :required="required === true" :id="`image-upload-${id}`" type="file" @change="previewImage" :accept="accept" class="uploader__input-field">
          <label class="uploader__input-upload-area" :for="`image-upload-${id}`">
            <svg class="svg-icon" viewBox="0 0 20 20">
							<path d="M4.317,16.411c-1.423-1.423-1.423-3.737,0-5.16l8.075-7.984c0.994-0.996,2.613-0.996,3.611,0.001C17,4.264,17,5.884,16.004,6.88l-8.075,7.984c-0.568,0.568-1.493,0.569-2.063-0.001c-0.569-0.569-0.569-1.495,0-2.064L9.93,8.828c0.145-0.141,0.376-0.139,0.517,0.005c0.141,0.144,0.139,0.375-0.006,0.516l-4.062,3.968c-0.282,0.282-0.282,0.745,0.003,1.03c0.285,0.284,0.747,0.284,1.032,0l8.074-7.985c0.711-0.71,0.711-1.868-0.002-2.579c-0.711-0.712-1.867-0.712-2.58,0l-8.074,7.984c-1.137,1.137-1.137,2.988,0.001,4.127c1.14,1.14,2.989,1.14,4.129,0l6.989-6.896c0.143-0.142,0.375-0.14,0.516,0.003c0.143,0.143,0.141,0.374-0.002,0.516l-6.988,6.895C8.054,17.836,5.743,17.836,4.317,16.411"></path>
						</svg> Upload file
          </label>
          <div class="loader" v-if="loading">
            <p class="loader__inner">Loading...</p>
          </div>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'

Vue.use(VueAxios, axios)

export default {
  name: 'Uploader',
  props: {
    value: {},
    required: {
      default: false
    },
    apiURL: {},
    accept: {
      default: 'image/*'
    },
    preview: {
      default: true
    }
  },
  data() {
    return {
      id: null,
      imageFile: null,
      imageInput: this.value,
      loading: false,
      errors: ''
    }
  },
  computed: {
    imageURL() {
      return this.value
    }
  },
  watch: {
    'value' () {
      this.imageInput = this.value
    },
    'imageInput' () {
      this.$emit('input', this.imageInput)
    }
  },
  methods: {
    // Image upload
    previewImage(event) {
      const input = event.target
      this.errors = []
      if (input.files && input.files[0] && input.files[0].size <= 1000000) {
        // Create a new FileReader to read this image and convert to base64 format
        const reader = new FileReader()
        const Vue = this

        reader.onload = (e) => {
          Vue.imageFile = input.files[0]
          this.uploadImage()
        }
        reader.readAsDataURL(input.files[0])
      } else {
        this.errors = 'File is to big!'
      }
    },
    uploadImage() {
      // Upload image if data exists, then update item
      let formData = new FormData()
      formData.append(this.apiURL, this.imageFile)
      this.loading = true
      this.$http.post(this.apiURL, formData).then(response => {
        this.imageInput = response.data.url
        this.$emit('input', response.data.url)
        this.loading = false
      }).catch(e => {
        this.errors = e.response.data
        this.loading = false
      })
    },
    removeImage() {
      this.imageInput = ''
    }
  },
  mounted() {
    this.id = this._uid
  }
}

</script>

<style scoped>
@keyframes rotate {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

.loader {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background: rgba(255, 255, 255, 0.7);
  z-index: 100;
}

.loader__inner {
  width: 20px;
  height: 20px;
  position: absolute;
  top: 50%;
  left: 50%;
  margin-left: -10px;
  margin-top: -10px;
  border: 2px solid #000;
  border-right: 2px solid transparent;
  border-radius: 50%;
  font-size: 0;
  animation: 500ms rotate infinite linear;
}

.uploader svg {
  fill: currentColor;
}

.uploader__input-wrapper {
  position: relative;
  padding-bottom: 50%;
  background: #DDD;
}

.uploader__input-wrapper:before {
  content: "";
  display: block;
  position: absolute;
  top: 5px;
  right: 5px;
  bottom: 5px;
  left: 5px;
  border: 2px dashed #888;
}

.uploader__input-field {
  position: absolute;
  opacity: 0;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  width: 100%;
  z-index: 100;
  cursor: pointer;
}

.uploader__input-wrapper:hover .uploader__input-remove {
  transform: scale(1);
  opacity: 1;
}

.uploader__input-wrapper:hover .uploader__input-upload-area {
  color: #888;
}

.uploader__input-upload-area {
  text-align: center;
  display: block;
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  color: #333;
  cursor: pointer;
  height: 60px;
  padding: calc(25% - 20px);
}

.uploader__input-upload-area svg {
  width: 24px;
  height: 24px;
  display: block;
  margin: 0 auto 10px auto;
}

.uploader__input-preview {
  background-size: cover;
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
}

.uploader__input-remove {
  all: unset;
  position: absolute;
  top: 50%;
  left: 50%;
  margin-top: -20px;
  margin-left: -20px;
  z-index: 10;
  text-align: center;
  display: block;
  background: #000;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  color: #FFF;
  transition: all 200ms, background 100ms;
  cursor: pointer;
  opacity: 0;
  transform: scale(0.5);
  outline: none;
}

.uploader__input-remove.is-persistent {
  transform: none;
  opacity: 1;
  margin-top: -35px;
}

.uploader__file-name {
  position: absolute;
  text-align: center;
  left: 0;
  right: 0;
  top: 50%;
  margin-top: 20px;
  color: #333;
}

.uploader__input-remove:hover {
  background: #333;
}

.uploader__input-remove svg {
  width: 18px;
  height: 18px;
  vertical-align: middle;
}

.uploader__note {
  flex: 1;
  color: #CCC;
  line-height: 1.3;
}
</style>
