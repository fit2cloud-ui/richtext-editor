<script lang="ts" setup>
import "@halo-dev/richtext-editor/dist/style.css";
import { computed, watchEffect, markRaw } from "vue";
import { unified } from "unified";
import rehypeParse from "rehype-parse";
import rehypeFormat from "rehype-format";
import rehypeStringify from "rehype-stringify";
import { useLocalStorage } from "@vueuse/core";
import {
  ExtensionBlockquote,
  ExtensionBold,
  ExtensionBulletList,
  ExtensionCode,
  ExtensionDocument,
  ExtensionDropcursor,
  ExtensionGapcursor,
  ExtensionHardBreak,
  ExtensionHeading,
  ExtensionHistory,
  ExtensionHorizontalRule,
  ExtensionItalic,
  ExtensionOrderedList,
  ExtensionStrike,
  ExtensionText,
  ExtensionImage,
  ExtensionTaskList,
  ExtensionLink,
  ExtensionTextAlign,
  ExtensionUnderline,
  ExtensionTable,
  ExtensionSubscript,
  ExtensionSuperscript,
  ExtensionPlaceholder,
  ExtensionHighlight,
  ExtensionCommands,
  ExtensionIframe,
  ExtensionVideo,
  ExtensionAudio,
  ExtensionCodeBlock,
  ExtensionColor,
  ExtensionFontSize,
  lowlight,
  RichTextEditor,
  useEditor,
  ExtensionIndent,
  ExtensionDraggable,
  ExtensionColumns,
  ExtensionColumn,
  ExtensionNodeSelected,
  ExtensionTrailingNode,
} from "@halo-dev/richtext-editor";

const content = useLocalStorage("content", "");

const editor = useEditor({
  content: content.value,
  extensions: [
    ExtensionBlockquote,
    ExtensionBold,
    ExtensionBulletList,
    ExtensionCode,
    ExtensionDocument,
    ExtensionDropcursor.configure({
      width: 2,
      class: "dropcursor",
      color: "skyblue",
    }),
    ExtensionGapcursor,
    ExtensionHardBreak,
    ExtensionHeading,
    ExtensionHistory,
    ExtensionHorizontalRule,
    ExtensionItalic,
    ExtensionOrderedList,
    ExtensionStrike,
    ExtensionText,
    ExtensionImage.configure({
      HTMLAttributes: {
        loading: "lazy",
      },
    }),
    ExtensionTaskList,
    ExtensionLink.configure({
      autolink: false,
      openOnClick: false,
    }),
    ExtensionTextAlign.configure({
      types: ["heading", "paragraph"],
    }),
    ExtensionUnderline,
    ExtensionTable.configure({
      resizable: true,
    }),
    ExtensionSubscript,
    ExtensionSuperscript,
    ExtensionPlaceholder.configure({
      placeholder: "输入 / 以选择输入类型",
    }),
    ExtensionHighlight,
    ExtensionVideo,
    ExtensionAudio,
    ExtensionCommands,
    ExtensionCodeBlock.configure({
      lowlight,
    }),
    ExtensionIframe,
    ExtensionColor,
    ExtensionFontSize,
    ExtensionIndent,
    ExtensionDraggable,
    ExtensionColumns,
    ExtensionColumn,
    ExtensionNodeSelected,
    ExtensionTrailingNode,
  ],
  onUpdate: () => {
    content.value = editor.value?.getHTML() + "";
  },
});

const formatContent = computed(() => {
  return unified()
    .use(rehypeParse)
    .use(rehypeFormat)
    .use(rehypeStringify)
    .processSync(content.value);
});

watchEffect(() => {
  console.log(String(formatContent.value));
});

const locales = [
  {
    code: "zh-CN",
    label: "中文",
  },
  {
    code: "en-US",
    label: "English",
  },
];

const locale = useLocalStorage("locale", "zh-CN");
</script>

<template>
  <div style="height: 100vh" class="flex">
    <RichTextEditor v-if="editor" :editor="editor" :locale="locale" />
  </div>
</template>
