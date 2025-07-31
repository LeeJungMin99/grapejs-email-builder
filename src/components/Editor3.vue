<!-- EmailBuilder.vue -->
<template>
  <div class="editor-wrapper">
    <div id="blocks-panel" class="left-panel" />
    <div id="gjs" class="center-panel" />
  </div>
</template>

<script setup>
import 'grapesjs/dist/css/grapes.min.css'
import { onMounted } from 'vue'
import grapesjs from 'grapesjs'
import presetNewsletter from 'grapesjs-preset-newsletter'
import basicBlocks from 'grapesjs-blocks-basic'
import forms from 'grapesjs-plugin-forms'
import customCode from 'grapesjs-custom-code'

onMounted(() => {
  const editor = grapesjs.init({
    container: '#gjs',
    height: '100vh',
    width: 'auto',
    storageManager: false,
    plugins: [
      presetNewsletter,
      basicBlocks,
      forms,
      customCode
    ],
    pluginsOpts: {
      [presetNewsletter]: {
        mjml: true,
        blocks: ['sect100', 'sect50', 'text', 'image', 'button', 'divider'],
      },
    },
    blockManager: { appendTo: '#blocks-panel' },
  /*   styleManager: { appendTo: '#style-panel' },
    traitManager: { appendTo: '#trait-panel' }, */
  })

  editor.on('load', () => {
    editor.Panels.removeButton('views', 'open-blocks');

  const body = editor.getWrapper();

  // 바디 스타일 초기 설정 (센터 정렬, 최대 너비)
  body.setStyle({
    'max-width': '600px',
    'margin': '0 auto',
    'padding': '20px',
    'background-color': '#ffffff',
  });

   const mjmlOutput = editor.getMjml?.();
  console.log('getMjml 결과:', mjmlOutput);
console.log(editor.Commands.getAll());
  // const panel = editor.Panels;
  // panel.removeButton('options', 'fullscreen'); //풀스크린 제거

   // 클래스 지정도 가능
  //body.addClass('custom-body-wrapper');
});
})
</script>

<style scoped>
.editor-wrapper {
  display: grid;
  grid-template-columns: 240px 1fr;
  height: 100vh;
  width:100%
}

.left-panel {
  background: #f4f4f4;
  overflow-y: auto;
  border-right: 1px solid #ddd;
}

.center-panel {
  background: #fff;
  overflow: auto;
}
.panel-left {
  width: 240px;
  border-right: 1px solid #ccc;
  overflow-y: auto;
  max-height: 100vh;
  scrollbar-width: thin;         /* Firefox */
  scrollbar-color: #555 #1e1e1e; /* Firefox */
}

/* Chrome, Edge, Safari */
.left-panel::-webkit-scrollbar {
  width: 6px;
}

.left-panel::-webkit-scrollbar-track {
  background: #1e1e1e;
}

.left-panel::-webkit-scrollbar-thumb {
  background-color: #555;
  border-radius: 3px;
}
</style>
