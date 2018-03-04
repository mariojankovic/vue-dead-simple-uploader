# Vue dead simple uploader ☝️

[![NPM version](https://img.shields.io/npm/v/vue-dsu.svg?style=flat)](https://npmjs.com/package/vue-dsu) [![NPM downloads](https://img.shields.io/npm/dm/vue-dsu.svg?style=flat)](https://npmjs.com/package/vue-dsu)

![Vue DSU](https://media.giphy.com/media/3XByK1IZRxe39nAnlk/giphy.gif)

A simple vue upload component with HTML only drag and drop features. It has image preview functionality and uses axios to push your image to whatever endpoint you specify.

## Install

```bash
yarn add vue-dsu
# or
npm install vue-dsu --save
```

CDN: [UNPKG](https://unpkg.com/vue-dsu/) | [jsDelivr](https://cdn.jsdelivr.net/npm/vue-dsu/) (available as `window.VueSimpleUploader`)

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

## ContriBOOT 👢
- `yarn dev`: Run the dev environment
- `yarn build`: Build for npm

### Getting started
```bash
# Fork the repo
git clone git@github.com:$YOUR_NAME/vue-dead-simple-uploader.git && cd vue-dead-simple-uploader

yarn dev
yarn build
```

## License

MIT &copy; [mariojankovic](https://github.com/mariojankovic)
