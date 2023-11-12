<script>
import VueMarkdown from 'vue-markdown-render'
import turndown from 'turndown';
const td = new turndown();

export default {
  components: {
    VueMarkdown,
  },
  data() {
    return {
      markdown:"",
    }
  },
  methods: {
    saveFile() {
      const markdownComponent = this.$refs.markdownComponent;
      const renderedHTML = markdownComponent.$el.innerHTML;

      const markdown = td.turndown(renderedHTML);

      const blob = new Blob([markdown], { type: 'text/plain' });

      const a = document.createElement('a');
      a.href = window.URL.createObjectURL(blob);
      a.download = 'markdown_rendered_content.md';
      a.click();

      window.URL.revokeObjectURL(a.href);
      
    },
  }
} 

</script>

<template>

  <div class="editor">
    <h1>Markdown</h1>
    <textarea name="editor" v-model="markdown"></textarea>
  </div>
  
  <div class="output">
    <vue-markdown ref="markdownComponent" :source="markdown"></vue-markdown>
  </div>
<button @click="saveFile">click me</button>

</template>

<style scoped>

textarea {
  height: 800px;
  width: 700px;
  resize: none;
}

.output {
  background-color: white;
  height: 800px;
  width: 700px;
  margin-top: 10%;
  margin-left: 10%;
  resize: none;
}

</style>
