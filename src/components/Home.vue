<template>
  <div>
    <group-tag v-for="value in fakeBackendObject.values" :key="value.value"  v-model="value.tags" :tagOptions="tagOptions" :tagsObj="fakeBackendObject"></group-tag>
  </div>
</template>

<script>
import GroupTag from './GroupTag'
import fakeBackendObject from './resource/FakeBackendTagObject.js'

export default {
  components: {
    GroupTag
  },
  data () {
    return {
      fakeBackendObject,
      tagOptions: fakeBackendObject.tagOptions
    }
  }
}
</script>
