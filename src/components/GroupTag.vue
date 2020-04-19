<template>
  <div class="top-container">
    <div class="tag-flex-container">
        <tag v-for="tag in tags" :key="tag.id" @removeTag="removeTag" :tag="tag"></tag>
        <span class="tag-input-container">
            <input placeholder="Add tag" ref="tagInput" v-model="newTag" @keydown.enter.stop.prevent="pushTag(newTag)" @keydown="handleKeydown" type="text" size="1" class="tag-input">
            <span :class="['dropdown-arrow',{rotate180:isTagOptionsVisible}]" @click="toggleTagOptions">&#9660;</span>
        </span>
    </div>
    <ul class="tag-options" v-if="isTagOptionsVisible">
      <li v-for="tag in filteredTagOptions" @click="pushTag(tag.value)" :key="tag.id">{{tag.value}}</li>
    </ul>
  </div>
</template>

<script>
import Tag from './Tag'

export default {
  props: {
    tagOptions: {
      type: Array,
      default: () => []
    },
    value: {
      type: Array,
      default: () => []
    },
    tagsObj: {
      type: Object,
      required: true
    }
  },
  components: {
    Tag
  },
  data () {
    return {
      newTag: '',
      filteredTagOptions: [],
      isTagOptionsVisible: false
    }
  },
  computed: {
    tags: {
      get () {
        return this.value
      },
      set (newTags) {
        this.$emit('input', newTags)
      }
    }
  },
  watch: {
    value (newVal) {
      this.tags = this.value
    },
    newTag (newVal) {
      if (newVal === '') {
        this.filteredTagOptions = this.tagOptions
      } else if (this.tagOptions != null) {
        const filterOptions = []
        this.tagOptions.forEach(tag => {
          var patt = new RegExp(newVal)
          var res = patt.test(tag.value)
          if (res) {
            filterOptions.push(tag)
          }
        })
        this.filteredTagOptions = filterOptions
      }
    }
  },
  created () {
    window.addEventListener('click', this.close)
  },
  mounted () {
    this.filteredTagOptions = this.tagOptions
  },
  beforeDestroy () {
    window.removeEventListener('click', this.close)
  },
  methods: {
    updateTagOptions () {
      const tagOptions = []
      this.tagsObj.values.forEach(value => {
        const tagOptionsValues = tagOptions.map(tag => tag.value)
        value.tags.forEach(tag => {
          if (tagOptionsValues.indexOf(tag.value) === -1) {
            tagOptions.push(tag)
          }
        })
      })

      this.tagOptions.forEach((tag, index) => {
        if (tagOptions.map(t => t.value).indexOf(tag.value) === -1) {
          this.tagOptions.splice(index, 1)
        }
      })
    },
    close (e) {
      if (!this.$el.contains(e.target)) {
        this.isTagOptionsVisible = false
      }
    },
    toggleTagOptions () {
      this.isTagOptionsVisible = !this.isTagOptionsVisible
    },
    pushTag (newTag) {
      if (newTag === '') {
        return
      }
      if (this.tags.map(tag => tag.value).indexOf(newTag) === -1) {
        const newTagObj = {
          id: null,
          value: newTag
        }
        if (this.tagOptions === null) {
          this.tagOptions = []
        }
        if (this.tagOptions.map(tag => tag.value).indexOf(newTag) === -1) {
          this.tagOptions.push(newTagObj)
          this.filteredTagOptions = this.tagOptions
        }
        this.tags.push(newTagObj)
        this.newTag = ''
      }
      this.$refs.tagInput.focus()
    },
    removeTag (tag) {
      const index = this.tags.map(tag => tag.value).indexOf(tag.value)
      this.tags.splice(index, 1)
    },
    removeLastTag () {
      if (this.newTag === '') {
        if (this.tags.length === 0) {
          return
        }
        this.tags.pop()
      }
    },
    handleKeydown (e) {
      if (e.key === 'Backspace') {
        this.removeLastTag()
      }

      if (e.key === 'ArrowDown') {
        this.isTagOptionsVisible = !this.isTagOptionsVisible
      }
    }
  }
}
</script>

<style scoped>
.border-bottom {
    border-bottom: 1px solid black;
}

.top-container {
  position: relative;
  z-index: 0;
}
.tag-flex-container {
    display: flex;
    height: 24px;
    padding-left: 8px;
    flex-wrap: wrap;
    border: 1px solid #8b9396;
}
.tag-input-container {
    display: flex;
    flex-grow: 1;
    height: 22px;
    flex-shrink: 0;
    flex-basis: auto;
    min-width: 100px;
}
.tag-input {
    flex-grow: 1;
    flex-shrink: 0;
    flex-basis: auto;
    min-width: 100px;
    border: none;
    padding: 0px;
    font-size: 11px;
    height: 22px;
}

.tag-input:focus{
  outline: none;
}

.dropdown-arrow {
    padding-top: 2px;
    font-size: 12px;
    margin-right: 4px;
    cursor: pointer;
}

.rotate180 {
    transform: rotate(-180deg);
    padding-top: 4px;
}

.tags-update-btn {
    height: 20px;
    border: none;
    cursor: pointer;
    font-size: 11px;
    margin-left: 2px;
    margin-top: 4px;

}

.tag-options {
  position: relative;
  z-index: 2;
  list-style: none;
  text-align: left;
  padding-left: 8px;
  border: 1px solid;
  border-top: 0px;
  border-color: rgba(200,200,200,1);
  margin-top: 0px;
  border-color: rgba(var(--palette-neutral-20,200, 200, 200),1);
  font-size: 13px;
}

.tag-options > li {
  margin-top: 2px;
  margin-bottom: 2px;
}

.tag-options > li:hover {
  background-color: rgba(239,246,252,1);
}
</style>
