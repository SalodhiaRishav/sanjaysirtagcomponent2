<template>
  <div>
    <div class="row">
      <div class="col">
          <group-tag v-for="(value, index) in fakeBackendObject.values" :key="value.value" :tagIndex="index" v-model="value.tags" :tagOptions="tagOptions" :tagValue="value.value" :tagsObj="fakeBackendObject" @valueChanged="fakeBackendObject.values[index].value = $event"></group-tag>
      </div>
      <div class="col">
        <pre>{{fakeBackendObject}}</pre>
      </div>
    </div>
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
