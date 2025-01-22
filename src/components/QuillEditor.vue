<template>
  <div>
    <div id="editor-container"></div>
    <button id="copy-button">コピー</button>
  </div>
</template>

<script setup lang="ts">
import { onMounted } from "vue";
import Quill from "quill";
import "quill/dist/quill.snow.css"; // 必要なスタイルをインポート

onMounted(() => {
  const quill = new Quill("#editor-container", {
    theme: "snow", // テーマ: 'snow' または 'bubble'
    placeholder: "入力してください（最大500文字まで）",
    modules: {
      toolbar: [
        ["bold"],
        ["italic"],
        ["underline"],
        ["strike"],
        [{ color: [] }],
        [{ background: [] }],
        [{ list: "bullet" }],
        ["link"]
        // カスタムボタンを追加する箇所
        // ['customButton'],
      ]
      // formats: [
      //   'bold',
      //   'italic',
      //   'underline',
      //   'strike',
      //   'blockquote',
      //   'code-block',
      //   'header',
      //   'font',
      //   'size',
      //   'color',
      //   'background',
      //   'list',
      //   'bullet',
      //   'indent',
      //   'align',
      //   'direction',
      //   'link',
      //   'image',
      //   'video',
      //   'formula',
      //   'code',
      //   'script',
      //   'spacer',
      //   'highlight',
      //   'customButton', // ここで定義
      // ],
    },
    fontSizes: ["14px"]
  });

  // // カスタムボタンのイベントハンドラーをここに移動
  // quill?.getModule('toolbar').addHandler('customButton', () => {
  //   // ボタンをクリックしたときの処理
  //   console.log('カスタムボタンがクリックされました');
  //   // 例: フォーマットの変更
  //   quill?.format('color', 'red');
  // });

  const observer = new MutationObserver((mutations) => {
    mutations.forEach((mutation) => {
      if (mutation.type === "childList") {
        // 子要素が追加または削除された場合の処理
        console.log("エディタの内容が変更されました");
      }
    });
  });
  observer.observe(quill.root, { childList: true });

  const copyButton = document.getElementById("copy-button");
  copyButton?.addEventListener("click", () => {
    const selection = quill.getSelection();
    const delta = quill.getContents(selection?.index, selection?.length);

    // クリップボードにコピー
    navigator.clipboard
      .write([
        new ClipboardItem({
          "text/html": new Blob([quill.root.innerHTML], { type: "text/html" })
        })
      ])
      .then(() => {
        console.log("コピー成功");
        console.log("コピーした内容:", delta); // ここでコピーしたデルタを出力
        console.log("JSON形式で:", JSON.stringify(delta, null, 2)); // 読みやすく整形したJSONで出力
      })
      .catch((err) => {
        console.error("コピー失敗", err);
      });
  });
});
</script>

<style>
/* エディタのスタイルを必要に応じて調整 */
#editor-container {
  height: 300px;
  border: 1px solid #ccc;
}

.ql-custom-button {
  /* カスタムボタンのスタイル */
  background-color: blue;
  color: white;
  padding: 5px 10px;
}
</style>
