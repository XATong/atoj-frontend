<template>
  <Viewer :value="value" :locale="locale" :plugins="plugins" />
</template>

<script setup lang="ts">
import gfm from "@bytemd/plugin-gfm";
import highlight from "@bytemd/plugin-highlight";
import { withDefaults, defineProps } from "vue";
import { Viewer } from "@bytemd/vue-next";
// eslint-disable-next-line @typescript-eslint/ban-ts-comment
// @ts-ignore
import locale from "../locales/zh_Hans.json";
import math from "@bytemd/plugin-math";
// eslint-disable-next-line @typescript-eslint/ban-ts-comment
// @ts-ignore
import gfmLocale from "@bytemd/plugin-gfm/locales/zh_Hans.json";
// eslint-disable-next-line @typescript-eslint/ban-ts-comment
// @ts-ignore
import mathLocale from "@bytemd/plugin-math/locales/zh_Hans.json";

interface Props {
  value: string;
}

const plugins = [
  gfm({
    locale: gfmLocale,
  }),
  highlight(),
  math({
    locale: mathLocale,
  }),
  // Add more plugins here
];
const props = withDefaults(defineProps<Props>(), {
  value: () => "",
});
</script>
<style>
.bytemd-toolbar-icon.bytemd-tippy.bytemd-tippy-right:last-child {
  display: none;
}
.bytemd-fullscreen {
  z-index: 100;
}
</style>
