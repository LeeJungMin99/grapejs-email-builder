<template>
  <div id="editor-wrapper" style="display: flex; height: 100vh;">
    <div class="panel-left" style="width: 240px; border-right: 1px solid #ccc; overflow-y: auto; max-height: 100vh;"></div>
    <div id="gjs" style="flex: 1;"></div>
    <div class="panel-right" style="width: 300px; border-left: 1px solid #ccc;"></div>
  </div>
</template>

<script setup>
import { onMounted } from 'vue'
import grapesjs from 'grapesjs'
import presetNewsletter from 'grapesjs-preset-newsletter'
import basicBlocks from 'grapesjs-blocks-basic'
import forms from 'grapesjs-plugin-forms'
import customCode from 'grapesjs-custom-code'

onMounted(() => {
  const editor = grapesjs.init({
    container: '#gjs',
    height: '100%',
    width: '100%',
    storageManager: false,
    blockManager: {
      appendTo: '.panel-left',
    },
    styleManager: {
      appendTo: '.panel-right',
    },
    traitManager: {
      appendTo: '.panel-right',
    },
    panels: {
      defaults: [
        { id: 'left', el: '.panel-left' },
        { id: 'right', el: '.panel-right' },
      ],
    },
    plugins: [
      presetNewsletter,
      basicBlocks,
      forms,
      customCode
    ],
    pluginsOpts: {
      [presetNewsletter]: {
        blocks: ['sect100', 'sect50', 'text', 'image', 'button', 'divider'],
        mjml: true,
      },
    },
  })
})
</script>

<style lang="scss" scoped>
body, html {
  margin: 0;
  padding: 0;
  font-family: sans-serif;
}
</style>
