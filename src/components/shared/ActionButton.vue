<template>
  <b-button :variant="variant" @click="$emit('click')" :disabled="!canExecute || executing">
    <template v-if="iconSide !== 'left'">
      <slot></slot>
    </template>
    <template v-if="icon != null">
      <i class="fas" :class="iconClass"></i>
    </template>
    <template v-if="iconSide === 'left'">
      <slot></slot>
    </template>
  </b-button>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: "action-button",
  props: {
    variant: {
      type: String,
      default: "secondary"
    },
    icon: {
      type: String,
      default: null
    },
    iconExecuting: {
      type: String,
      default: "fa-spinner"
    },
    canExecute: {
      type: Boolean,
      default: true
    },
    executing: {
      type: Boolean,
      default: false
    },
    iconSide: {
      type: String,
      default: "left"
    }
  },
  computed: {
    iconClass(this: any) {
      const icon = this.$props.icon;
      const executing = this.$props.executing;
      const iconExecuting = this.$props.iconExecuting;

      return {
        ...(icon !== iconExecuting
          ? {
              [icon]: !executing,
              [iconExecuting]: executing
            }
          : {
              [icon]: true
            }),
        "fa-spin": executing,
        "mr-1": this.iconSide == "left",
        "ml-1": this.iconSide != "left"
      };
    }
  }
});
</script>

