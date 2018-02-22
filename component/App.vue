<template>
  <div id="app">
    <uploader accept="" :required="true" v-model="thumbnail_url"/>
  </div>
</template>

<script>
import Uploader from '../src/Uploader.vue'

export default {
  components: {
    Uploader
  },
  data () {
    return {
      thumbnail_url: 'https://picsum.photos/600/600'
    }
  }
}
</script>
