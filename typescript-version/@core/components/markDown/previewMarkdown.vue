<template>
    <MdPreview :id="id" :modelValue="text" />
    <MdCatalog :editorId="id" :scrollElement="scrollElement" />
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { MdPreview, MdCatalog } from 'md-editor-v3';
// preview.css相比style.css少了编辑器那部分样式
import 'md-editor-v3/lib/preview.css';
const text = ref('# Hello Editor');
const id = ref("my-editor");
let scrollElement: HTMLElement | string;
// 通过import.meta.env.SSR 判断 是否是ssr
if (import.meta.env.SSR) {
    scrollElement = "body";
} else {
    scrollElement = document.documentElement;
}
</script>