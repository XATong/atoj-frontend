<!-- <template>
  <Editor
    :value="value"
    :mode="mode"
    :plugins="plugins"
    @change="handleChange"
  />
</template>

<script setup lang="ts">
import gfm from "@bytemd/plugin-gfm";
import highlight from "@bytemd/plugin-highlight";
import math from "@bytemd/plugin-math";
import { Editor, Viewer } from "@bytemd/vue-next";
import { ref, withDefaults, defineProps } from "vue";

/**
 * 定义组件属性类型
 */
interface Props {
  value: string;
  mode?: string;
  handleChange: (v: string) => void;
}

const plugins = [
  gfm(),
  highlight(),
  math(),
  // Add more plugins here
];

/**
 * 给组件指定初始值
 */
const props = withDefaults(defineProps<Props>(), {
  value: () => "",
  mode: () => "split",
  handleChange: (v: string) => {
    console.log(v);
  },
});
</script>

<style>
.bytemd-toolbar-icon.bytemd-tippy.bytemd-tippy-right:last-child {
  display: none;
}
</style> -->

<template>
  <Editor
    class="mdEditor"
    :value="props.value"
    :locale="locale"
    :mode="props.mode"
    :plugins="plugins"
    @change="props.handleChange"
  />
</template>

<script setup lang="ts">
import gfm from "@bytemd/plugin-gfm";
import highlight from "@bytemd/plugin-highlight";
import { Editor } from "@bytemd/vue-next";
import { withDefaults, defineProps } from "vue";
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
  mode?: string;
  handleChange: (v: string) => void;
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
  mode: () => "split",
  handleChange: (v: string) => {
    console.log(v);
  },
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
