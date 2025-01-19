<template>
  <div>
    <div id="editor-container"></div>
  </div>
</template>

<script>
import { onMounted } from "vue";
import Quill from "quill";
import "quill/dist/quill.snow.css"; // 必要なスタイルをインポート

export default {
  setup() {
    onMounted(() => {
      const quill = new Quill("#editor-container", {
        theme: "snow", // テーマ: 'snow' または 'bubble'
        modules: {
          toolbar: [
            [{ header: [1, 2, false] }],
            ["bold", "italic", "underline"],
            [{ list: "ordered" }, { list: "bullet" }],
            ["link", "image"]
          ]
        }
      });

      // 必要に応じてエディタの内容を取得
      quill.on("text-change", () => {
        console.log("エディタ内容:", quill.root.innerHTML);
      });
    });
  }
};
</script>

<style>
/* エディタのスタイルを必要に応じて調整 */
#editor-container {
  height: 300px;
  border: 1px solid #ccc;
}
</style>
