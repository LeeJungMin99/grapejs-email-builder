<template>
  <div id="gjs"></div>
</template>

<script setup>
import { onMounted } from 'vue'
import grapesjs from 'grapesjs'
import presetNewsletter from 'grapesjs-preset-newsletter'
import basicBlocks from 'grapesjs-blocks-basic'
import forms from 'grapesjs-plugin-forms'
import customCode from 'grapesjs-custom-code'

// 🚨 반드시 CSS 포함해야 UI 보임!
import 'grapesjs/dist/css/grapes.min.css'

onMounted(() => {
  const editor = grapesjs.init({
    container: '#gjs',
    fromElement: false,
    height: '100vh',
    width: '100%',
    storageManager: false,
    plugins: [presetNewsletter,
    basicBlocks,
    forms,
    customCode,
    ],
    pluginsOpts: {
      'grapesjs-preset-newsletter': {
        mjml: true,
      },
    },
  })

  editor.on('load', () => {
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

console.log("editor.getWrapper().components()" , editor.getWrapper().components())

const wrapper = editor.getWrapper();
console.log(
  wrapper.components().map(c => ({
    type: c.get('type'),
    tagName: c.get('tagName')
  }))
);


editor.Panels.addButton('options', [{
  id: 'view-code',
  className: 'fa fa-code',
  label: '코드',
  command: 'open-code-view'
}]);

// 예: 커스텀 팝업 명령어 등록
editor.Commands.add('open-code-view', {
  run(editor) {
    const html = editor.getHtml();
    const css = editor.getCss();
    const fullHtml = `
      <!DOCTYPE html>
      <html>
        <head><style>${css}</style></head>
        <body>${html}</body>
      </html>
    `;

    editor.Modal.open({
      title: '소스 보기',
      content: `
        <select id="code-select">
          <option value="html">HTML</option>
          <option value="mjml">MJML</option>
        </select>
        <textarea style="width:100%; height:400px;" id="code-area">${fullHtml}</textarea>
      `
    });

    // 선택 박스 이벤트 처리
    setTimeout(() => {
      document.getElementById('code-select').addEventListener('change', (e) => {
        const type = e.target.value;
        if (type === 'html') {
          document.getElementById('code-area').value = fullHtml;
        } else if (type === 'mjml') {
          const mjml = editor.getMjml?.(); // preset-newsletter 등에서 제공
          document.getElementById('code-area').value = mjml || 'MJML 출력 불가';
        }
      });
    });
  }
});

/* editor.BlockManager.add('resizable-button', {
  label: '🔲 크기조절 버튼',
  content: {
    tagName: 'button',
    content: 'Resize Me',
    attributes: { class: 'resizable-btn' },
    style: {
      width: '150px',
      height: '50px',
      display: 'inline-block',
      backgroundColor: '#007bff',
      color: '#fff',
      border: 'none',
      borderRadius: '5px',
      cursor: 'pointer',
    },
    resizable: {
      cr: 1, // center-right
      br: 1, // bottom-right
      minDim: 20,
    },
  }
}); */

editor.DomComponents.addType('resizable-button', {
  model: {
    defaults: {
      tagName: 'button',
      content: 'Resize me',
      resizable: { cr: 1, br: 1 },
      style: {
        width: '160px',
        height: '60px',
        display: 'inline-block',
        background: '#28a745',
        color: 'white',
        border: 'none',
        borderRadius: '4px',
      },
    }
  }
});

editor.BlockManager.add('resizable-button', {
  label: '<svg viewBox="0 0 24 24"><path fill="currentColor" d="M20 20.5C20 21.3 19.3 22 18.5 22H13C12.6 22 12.3 21.9 12 21.6L8 17.4L8.7 16.6C8.9 16.4 9.2 16.3 9.5 16.3H9.7L12 18V9C12 8.4 12.4 8 13 8S14 8.4 14 9V13.5L15.2 13.6L19.1 15.8C19.6 16 20 16.6 20 17.1V20.5M20 2H4C2.9 2 2 2.9 2 4V12C2 13.1 2.9 14 4 14H8V12H4V4H20V12H18V14H20C21.1 14 22 13.1 22 12V4C22 2.9 21.1 2 20 2Z"></path></svg><div>Button</div>',
  content: { type: 'resizable-button' },
  title:'Button'
});
  /* editor.BlockManager.remove('button'); // 기존 버튼 제거

editor.BlockManager.add('centered-button', {
  label: '중앙 버튼',
  attributes: { class: 'gjs-block gjs-f-button' },  // 기존 버튼과 같은 아이콘
  category: '기본',
  content: `
    <div class="gjs-button-container" style="text-align: center;">
      <a href="#" class="gjs-button">중앙 버튼</a>
    </div>
  `,
}); */


})

</script>

<style lang="scss">
#gjs {
  height: 100%;
  width:100%;
  background-color: #f0f0f0; /* 배경 안보이는 문제 방지 */
  overflow: hidden;

  .gjs-toolbar { /*툴바 위치 조정하기...  */
    top: auto !important;
    bottom: -35px !important;
    }

    /* 좌측 패널 (블록 등) */
    .gjs-pn-panel {
    background-color: #000 !important;
    color: #fff !important;
    }

    /* 툴바 아이콘 색상 */
    .gjs-pn-btn, .gjs-toolbar-item, .gjs-pn-buttons .fa {
    color: #ccc !important; 
    }

    /* 블록 스타일 */
    .gjs-block {
    background-color: #111 !important;
    border-color: #444 !important;
    color: #eee !important;
    }

}

</style>
