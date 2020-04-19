<template>
  <div class="tag">
    <span class="tag-text">{{tag.value}} <span class="tag-cross" @click = "removeTag(tag)">X</span></span>
  </div>
</template>

<script>
export default {
  props: {
    tag: {
      type: Object,
      required: true
    }
  },
  methods: {
    removeTag (tag) {
      this.$emit('removeTag', tag)
    }
  }
}
</script>

<style scoped>
.tag {
    background-color: rgba(239,246,252,1);
    padding: 4px;
    text-align: center;
    color: rgba(0,0,0,.55);
    margin-top: 4px;
    margin-bottom: 4px;
    margin-right: 12px;
    line-height: 4px;
}
.tag-text {
  font-size: 12px;
}

.tag-cross {
  cursor: pointer;
  font-size: 9px;
  margin-left: 8px;
}

.tag-cross:hover {
  color: black;
  font-size: 10px;
}

</style>
