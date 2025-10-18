<template>
  <div class="editor-container">
    <div class="line-numbers" ref="lineNumbers">
      <div v-for="n in lineCount" :key="n" class="line-number">{{ n }}</div>
    </div>

    <textarea
      v-model="markdown"
      placeholder="Write here your markdown code..."
      @input="onInput"
      @keydown="onKeyDown"
      @scroll="syncScroll"
      ref="textarea"
    ></textarea>
  </div>
</template>

<script>
export default {
  name: 'MdArea',
  props: {
    modelValue: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      markdown: this.modelValue
    }
  },
  computed: {
    lineCount() {
      return this.markdown.split('\n').length || 1
    }
  },
  watch: {
    modelValue(newVal) {
      if (newVal !== this.markdown) {
        this.markdown = newVal
      }
    }
  },
  methods: {
    onInput() {
      this.$emit('update:modelValue', this.markdown)
    },
    syncScroll() {
      const scrollTop = this.$refs.textarea.scrollTop
      this.$refs.lineNumbers.style.transform = `translateY(-${scrollTop}px)`
    },
    onKeyDown(event) {
      if (event.key === 'Tab') {
        event.preventDefault()
        const textarea = this.$refs.textarea
        const start = textarea.selectionStart
        const end = textarea.selectionEnd
        const tabSpaces = '\t'
        this.markdown =
          this.markdown.substring(0, start) +
          tabSpaces +
          this.markdown.substring(end)
        this.$nextTick(() => {
          textarea.selectionStart = textarea.selectionEnd = start + tabSpaces.length
          this.onInput() 
        })
      }
    }
  },
  mounted() {
    this.$refs.textarea.addEventListener('scroll', this.syncScroll)
  },
  beforeUnmount() {
    this.$refs.textarea.removeEventListener('scroll', this.syncScroll)
  }
}
</script>

<style scoped>
.editor-container {
  display: flex;
  height: 100%;
  width: 100%;
  background: var(--panel-bg);
  color: var(--panel-fg);
  font-family: monospace;
  font-size: 1rem;
  overflow: hidden;
  position: relative;
  margin: 0;
}

.line-numbers {
  padding: 1rem 0.5rem;
  background: var(--line-number-bg, var(--border-color));
  text-align: right;
  user-select: none;
  min-width: 3rem;
  border-right: 1px solid var(--border-color);
  overflow: hidden;
  position: relative;
  color: var(--text-secondary);
  margin-left: none;
}

.line-number {
  height: 1.5em;
  line-height: 1.5em;
}

textarea {
  width: 100%;
  height: 100%;
  padding: 1rem;
  border: none;
  background: transparent;
  color: var(--panel-fg);
  font-family: inherit;
  font-size: inherit;
  line-height: 1.5em;
  resize: none;
  outline: none;
  box-sizing: border-box;
  overflow-y: auto;
}
</style>
