# Vue dead simple uploader ☝️

[![NPM version](https://img.shields.io/npm/v/vue-dead-simple-uploader.svg?style=flat)](https://npmjs.com/package/vue-dead-simple-uploader) [![NPM downloads](https://img.shields.io/npm/dm/vue-dead-simple-uploader.svg?style=flat)](https://npmjs.com/package/vue-dead-simple-uploader)

A simple vue upload component with HTML only drag and drop features. It has image preview functionality and uses axios to push your image to whatever endpoint you specify.

## Install

```bash
yarn add vue-dsu
```

CDN: [UNPKG](https://unpkg.com/vue-dead-simple-uploader/) | [jsDelivr](https://cdn.jsdelivr.net/npm/vue-dead-simple-uploader/) (available as `window.VueSimpleUploader`)

## Installation
```javascript
import Uploader from 'vue-dsu'
Vue.use(Uploader)
```

## Props
```javascript
required: { // Field is not mandatory by default
  default: false
},
apiURL: {}, // http://your-api-url.com/
accept: {
  default: 'image/*'
},
preview: { // Show image preview by default
  default: true
}
```

## Usage
```vue
<template>
  <uploader v-model="thumbnail_url" :required="true" apiURL="http://yourapiurl.com" accept="" />
</template>

<script>
data () {
  return {
    thumbnail_url: 'https://picsum.photos/300/300'
  }
}
</script>
```

## License

MIT &copy; [mariojankovic](https://github.com/mariojankovic)
