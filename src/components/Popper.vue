<template>
  <slot name="trigger" v-bind="{ show, hide }" />
  <Teleport to="body">
    <div
      ref="popover"
      :data-show="isVisible"
      :data-initialized="popperInstance !== null"
      :class="$style.root"
    >
      <div :class="$style.arrow" data-popper-arrow />
      <div :class="$style.content">
        <slot />
      </div>
    </div>
  </Teleport>
</template>

<script lang="ts" setup>
import { createPopper } from '@popperjs/core'
import { defineProps } from '@vue/runtime-core';

const props = defineProps({
  placement: {
    type: String,
    default: 'auto'
  },
  strategy: {
    type: String,
    default: 'absolute'
  }
})

ref: isVisible = false
ref: popover = null
ref: popperInstance = null as any

function createPopover (target: HTMLElement) {
  popperInstance = createPopper(
    target as unknown as HTMLElement,
    popover as unknown as HTMLElement,
    {
      strategy: props.strategy as any,
      placement: props.placement as any,
      modifiers: [
        {
          name: 'arrow',
          options: {
            padding: 24
          }
        },
        {
          name: 'offset',
          options: {
            offset: [0, 18]
          }
        }
      ]
    }
  )
}

function destroyPopover () {
  popperInstance?.destroy()
  popperInstance = null;
}

function show (target: EventTarget|null) {
  isVisible = true
  createPopover(target as unknown as HTMLElement)
}

function hide () {
  isVisible = false
  destroyPopover()
}
</script>

<style lang="scss" module>
.root {
  --background-color: white;
  --border-color: lightgray;

  display: none;
  background-color: var(--background-color);
  border: 1px solid var(--border-color);
  border-radius: 10px;
  pointer-events: none;
  opacity: 0;
  z-index: 999;
}

.arrow,
.arrow::before {
  width: 0;
  height: 0;
  border-style: solid;
}

.arrow::before {
  content: '';
  position: absolute;
}

.root:global([data-show="true"]) {
  opacity: 1;
  pointer-events: initial;
}

.root:global([data-initialized]) {
  display: block;
}

.root:global([data-popper-placement^="bottom"]) {
  .arrow {
    top: -10px;
    border-width: 0 8px 10px 8px;
    border-color: transparent transparent var(--border-color) transparent;
    margin-left: -8px;

    &::before {
      top: 1px;
      left: -7px;
      border-width: 0 7px 9px 7px;
      border-color: transparent transparent var(--background-color) transparent;
    }
  }
}

.root:global([data-popper-placement^="top"]) {
  .arrow {
    bottom: -10px;
    border-width: 10px 8px 0 8px;
    border-color: var(--border-color) transparent transparent transparent;
    margin-left: -8px;

    &::before {
      bottom: 1px;
      left: -7px;
      border-width: 9px 7px 0 7px;
      border-color: var(--background-color) transparent transparent transparent;
    }
  }
}

.root:global([data-popper-placement^="left"]) {
  .arrow {
    right: -10px;
    margin-top: -8px;
    border-width: 8px 0 8px 10px;
    border-color: transparent transparent transparent var(--border-color);

    &::before {
      right: 1px;
      top: -7px;
      border-width: 7px 0 7px 9px;
      border-color: transparent transparent transparent var(--background-color);
    }
  }
}

.root:global([data-popper-placement^="right"]) {
  .arrow {
    left: -10px;
    margin-top: -8px;
    border-width: 8px 10px 8px 0;
    border-color: transparent var(--border-color) transparent transparent;

    &::before {
      left: 1px;
      top: -7px;
      border-width: 7px 9px 7px 0;
      border-color: transparent var(--background-color) transparent transparent;
    }
  }
}

.content {
  padding: 16px;
}
</style>

