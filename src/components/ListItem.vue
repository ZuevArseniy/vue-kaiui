<template>
  <div
    class="kaiui-listitem"
    v-bind:nav-selectable="true"
    tabindex="0"
    v-on:focus="handleFocusChange(true)"
    v-on:blur="handleFocusChange(false)"
    v-on:click="onClick"
  >
    <span class="kaiui-listitem-icon" v-bind:class="iconLeft" v-if="$slots.iconLeft || hasLeftIcon">
        <slot name="iconLeft" v-if="!hasLeftIcon"></slot>
    </span>
    <div class="kaiui-listitem-text-wrapper">
      <span class="kaiui-p_pri kaiui-listitem-primary-text">{{
        primaryText
      }}</span>
      <span class="kaiui-p_sec kaiui-listitem-secondary-text">{{
        secondaryText
      }}</span>
      <span class="kaiui-p_thi kaiui-listitem-tertiary-text">{{
        tertiaryText
      }}</span>
    </div>
    <span class="kaiui-listitem-icon right" v-bind:class="iconRight" v-if="$slots.iconRight || hasRightIcon">
        <slot name="iconRight" v-if="!hasRightIcon"></slot>
    </span>
  </div>
</template>

<script>
/**
 * The `<kaiui-list-item>` component.
 *
 * @author Sebastian Baar
 * @license MIT
 */

export default {
  name: "kaiui-list-item",
  props: {
    /**
     * The Softkeys Object
     * @type {{ left: String, center: String, right: String }}
     * @default { center: "Select" }
     */
    softkeys: {
      default: () => ({ center: "Select" }),
      type: Object,
      required: false,
    },
    /**
     * The Primary Text
     */
    primaryText: {
      type: String,
      required: true,
    },
    /**
     * The Secondary Text
     */
    secondaryText: {
      type: String,
      required: false,
    },
    /**
     * The Tertiary Text
     */
    tertiaryText: {
      type: String,
      required: false,
    },
    /**
     * The Left Icon CSS class
     */
    iconLeft: {
      default: "none",
      type: String,
      required: false,
    },
    /**
     * The Right Icon CSS class
     */
    iconRight: {
      default: "none",
      type: String,
      required: false,
    },
  },
  mounted() {
    this.$on("softkey-left-pressed", () => {
      /**
       * Emit the event `softLeft` when left softkey is selected
       */
      this.$emit("softLeft");
    });
    this.$on("softkey-right-pressed", () => {
      /**
       * Emit the event `softRight` when right softkey is selected
       */
      this.$emit("softRight");
    });
    this.$on("softkey-center-pressed", () => {
      /**
       * Emit the event `softCenter` when center softkey is selected
       */
      this.$emit("softCenter");
    });
  },
  computed: {
    hasLeftIcon() {
      return this.iconLeft && this.iconLeft != 'none';
    },
    hasRightIcon() {
      return this.iconRight && this.iconRight != 'none';
    },
  },
  methods: {
    /**
     * @private
     */
    handleFocusChange(isNowFocused) {
      if (isNowFocused) {
        /**
         * @private
         */
        this.$root.$emit("update-softkeys-register", this);
      } else {
        /**
         * @private
         */
        this.$root.$emit("update-softkeys-unregister");
      }
    },
    /**
     * @private
     */
    onClick() {
      this.handleFocusChange(true);
      /**
       * @private
       */
      this.$root.$emit("set-element-selected", this.$el);
    },
  },
};
</script>

<style>
.kaiui-listitem {
  cursor: pointer;
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  min-height: 60px;
  max-height: 60px;
  padding: 0 10px;
  text-overflow: ellipsis;
  overflow: hidden;
  outline: 0;
}
.kaiui-listitem[nav-selected="true"] {
  background-color: var(--listitem-selected-background-color);
}

.kaiui-listitem .kaiui-listitem-text-wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  overflow: hidden;
  flex: 1;
}

.kaiui-listitem .kaiui-listitem-text-wrapper span {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.kaiui-listitem[nav-selected="true"] .kaiui-listitem-primary-text {
  color: var(--listitem-selected-text-color);
}
.kaiui-listitem[nav-selected="true"] .kaiui-listitem-secondary-text {
  color: var(--listitem-selected-text-color);
}
.kaiui-listitem[nav-selected="true"] .kaiui-listitem-tertiary-text {
  color: var(--listitem-selected-text-color);
}
.kaiui-listitem[nav-selected="true"] .kaiui-listitem-icon {
  color: var(--listitem-selected-text-color);
}
.kaiui-listitem[nav-selected="true"] .kaiui-listitem-icon.right {
  color: var(--listitem-selected-text-color);
}

.kaiui-listitem .kaiui-listitem-icon {
  display: flex;
  align-items: center;
  margin-right: 10px;
  flex-shrink: 0;
}

.kaiui-listitem .kaiui-listitem-icon.right {
  margin-left: auto;
  margin-right: 0;
  font-size: 20px;
  flex-shrink: 0;
  margin-left: 10px;
}
.kaiui-listitem .kaiui-listitem-icon.right {
  color: var(--listitem-icon-right-color);
}
</style>
