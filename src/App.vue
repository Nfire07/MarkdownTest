<script>
import MdArea from './components/mdArea.vue'
import MarkdownIt from 'markdown-it'
import markdownItKatex from 'markdown-it-katex'
import 'katex/dist/katex.min.css'
import hljs from 'highlight.js'
import 'highlight.js/styles/github-dark.css'
import html2pdf from 'html2pdf.js'

const md = new MarkdownIt({
  html: true,
  breaks: true,
  highlight: function (str, lang) {
    if (lang && hljs.getLanguage(lang)) {
      try {
        return `<pre class="hljs"><code>${hljs.highlight(str, { language: lang, ignoreIllegals: true }).value}</code></pre>`
      } catch (__) {}
    }
    return `<pre class="hljs"><code>${md.utils.escapeHtml(str)}</code></pre>`
  }
}).use(markdownItKatex)

export default {
  name: 'App',
  components: { MdArea },
  data() {
    return {
      markdown: '# Hello World!'
    }
  },
  computed: {
    renderedMarkdown() {
      return md.render(this.markdown)
    }
  },
  methods: {
    exportToPDF() {
      const element = this.$refs.preview

      element.classList.add('light-mode')

      const opt = {
        margin: 0.5,
        filename: 'documento.pdf',
        image: { type: 'jpeg', quality: 0.98 },
        html2canvas: { scale: 2 },
        jsPDF: { unit: 'in', format: 'a4', orientation: 'portrait' }
      }

      html2pdf()
        .set(opt)
        .from(element)
        .save()
        .then(() => {
          element.classList.remove('light-mode')
        })
        .catch(() => {
          element.classList.remove('light-mode')
        })
    }
  }
}
</script>

<template>
  <div class="page">
    <div class="top-bar">
      <button @click="exportToPDF" class="export-button">Export as PDF</button>
    </div>
    <div class="content">
      <div class="editor">
        <MdArea v-model="markdown" />
      </div>
      <div class="preview" ref="preview">
        <div v-html="renderedMarkdown"></div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.page {
  display: flex;
  flex-direction: column;
  width: 100vw;
  height: 100vh;
  background-color: var(--main-bg);
  color: var(--fg);
  overflow: hidden;
}

.top-bar {
  position: sticky;
  top: 0;
  z-index: 10;
  height: 3rem;
  width: 100%;
  background-color: var(--panel-bg);
  border-bottom: 1px solid var(--border-color);
  display: flex;
  align-items: center;
  padding: 0 1rem;
  box-sizing: border-box;
}

.export-button {
  padding: 0.5rem 1rem;
  background-color: var(--button-bg);
  color: var(--button-fg);
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s ease;
}

.export-button:hover {
  background-color: var(--button-hover-bg);
}

.export-button:active {
  background-color: var(--button-active-bg);
}

.content {
  display: flex;
  flex: 1;
  height: calc(100vh - 3rem);
  width: 100%;
  overflow: hidden;
}

.editor {
  width: 50%;
  padding: 1rem;
  box-sizing: border-box;
  background-color: var(--panel-bg);
  color: var(--panel-fg);
  border-right: 1px solid var(--border-color);
  overflow: hidden;
}

.preview {
  width: 50%;
  padding: 1rem;
  box-sizing: border-box;
  background-color: var(--main-bg);
  color: var(--fg);
  font-family: system-ui, -apple-system;
  overflow-y: auto;
}

.hljs {
  background: var(--highlight-bg, #282c34);
  padding: 1rem;
  border-radius: 5px;
  overflow-x: auto;
  color: var(--highlight-fg, white);
}

::v-deep .katex {
  font-size: 1.1em;
  display: inline-block;
  vertical-align: middle;
}

.preview.light-mode {
  background-color: white !important;
  color: black !important;
}

.preview.light-mode .hljs {
  background: #f0f0f0 !important;
  color: #333 !important;
}

.preview.light-mode ::v-deep .katex {
  color: black !important;
}

.preview.light-mode a {
  color: blue !important;
}

.preview.light-mode h1, 
.preview.light-mode h2, 
.preview.light-mode h3, 
.preview.light-mode h4, 
.preview.light-mode h5, 
.preview.light-mode h6 {
  color: black !important;
}
</style>
