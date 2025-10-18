# Markdown Editor with Live Preview and PDF Export

This is a Vue 3-based Markdown editor that allows you to write, test, and preview `.md` files in real-time. It supports:

- Live Markdown rendering
- LaTeX rendering via KaTeX
- Syntax highlighting for code blocks using highlight.js
- Light and dark themes
- PDF export with automatic theme adjustments
- Line numbering in the editor
- Keyboard support for tab insertion and synchronized scrolling

---

## Features

- **Markdown Editor**
  - Real-time preview
  - Syntax highlighting for supported languages
  - Full KaTeX support for inline and block-level LaTeX
  - Line numbering
  - Keyboard tab insertion support
- **PDF Export**
  - Generates a print-ready PDF using the light theme
  - Retains KaTeX rendering and code highlighting
- **Customizable Styles**
  - Easily switch or define light/dark themes via CSS variables

---

## Technology Stack

| Technology       | Description                         |
|------------------|-------------------------------------|
| Vue 3            | Front-end framework                 |
| Markdown-it      | Markdown parser                     |
| markdown-it-katex| KaTeX plugin for LaTeX support      |
| highlight.js     | Syntax highlighting for code blocks |
| html2pdf.js      | PDF export from HTML content        |

---

## Installation

Clone the repository and install the dependencies:

```bash
git clone https://github.com/your-username/markdown-editor-vue.git
cd markdown-editor-vue
npm install
npm run dev
