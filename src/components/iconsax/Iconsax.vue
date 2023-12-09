<script setup lang="ts">
import { useSlots, computed, h } from "vue";
import { getIcon as getOutlineIcon } from "./icons/outline";
import { getIcon as getBoldIcon } from "./icons/bold";
import { getIcon as getBulkIcon } from "./icons/bulk";
import { getIcon as getTwotoneIcons } from "./icons/twotone";
import type { IconType } from "./type";
const icons = {
  bold: getBoldIcon,
  outline: getOutlineIcon,
  bulk: getBulkIcon,
  twotone: getTwotoneIcons,
};
interface Props {
  size?: string | number;
  color?: string | undefined;
  small?: boolean;
  xSmall?: boolean;
  type?: keyof typeof icons;
  solid?: boolean;
  strokeWidth?: string | null;
  icon?: IconType;
  rotate?: "rotate-90" | "rotate-270" | "rotate-180";
}
const props = withDefaults(defineProps<Props>(), {
  size: "20px",
  small: false,
  xSmall: false,
  type: "outline",
  solid: false,
  strokeWidth: "0.5",
});
const slot = useSlots();
const iconName = computed<string>(() => {
  if (props.icon) {
    return props.icon;
  }
  return slot.default ? (slot.default()[0].children as string).trim() ?? "" : "";
});
const iconPureName = computed(() => {
  if (iconName.value && icons[props.type]) {
    const i = iconName.value as IconType;
    return icons[props.type](i);
  }
  return null;
});
const iconElement = computed(() => {
  if (!iconPureName.value) {
    return;
  }
  return h("svg", {
    style: {
      strokeWidth: ["bulk", "bold"].includes(props.type)
        ? undefined
        : props.strokeWidth
        ? props.strokeWidth
        : "2px",
      fill: "none",
      color: props.color,
    },
    xmlns: "http://www.w3.org/2000/svg",
    // class: "mb-n2",
    viewBox: `0 0 24 24`,
    innerHTML: iconPureName.value,
    stroke: props.strokeWidth === "0" ? undefined : "currentColor",
  });
});

const iconSize = computed(() => {
  if (props.small) return "20px";
  else if (props.xSmall) return "15px";
  else if (props.size) {
    if (!Number(props.size)) {
      if (props.size === "x-small") return "15px";
      else if (props.size === "small") return "20px";
      return props.size;
    } else return `${props.size}px`;
  }
  return "10px";
});
const camelToDashCase = (str: string) =>
  str.replace(/[A-Z]/g, (letter) => `-${letter.toLowerCase()}`);
</script>
<template>
  <span class="icon" :style="{ color, width: iconSize, height: iconSize }">
    <template v-if="iconElement">
      <KeepAlive>
        <component
          :class="[
            !!props.rotate && props.rotate,
            slot.default
              ? camelToDashCase(String(slot.default()[0].children).trim())
              : '',
          ]"
          style="width: inherit; height: inherit; color: inherit"
          :is="iconElement"
        />
      </KeepAlive>
    </template>
  </span>
</template>
<style scoped>
.rotate-90 {
  transform: rotate(90deg);
}
.rotate-180 {
  transform: rotate(180deg);
}
.rotate-270 {
  transform: rotate(270deg);
}
</style>
./icons/bold./icons/bulk ./icons/twotone./icons/outline ./icon/outline
./icons/outline./icons/bold./icons/bulk./icons/twotone
