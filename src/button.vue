<template>
  <label class="switch-label" :style="labelStyle">
    <input type="checkbox" class="switch-input" @change.stop="toggle">
    <div class="switch-circle" :style="circleStyle"></div>
    <template v-if="label">
      <span v-if="active"
        class="switch-label-c switch-left"
        :style="switchLabelStyle"
        v-html="labelChecked">
      </span>
      <span v-else
        class="switch-label-c switch-right"
        :style="switchLabelStyle"
        v-html="labelUnchecked">
      </span>
    </template>
  </label>
</template>

<script>
const constants = {
  width: 50,
  height: 20,
  margin: 2,
  labelChecked: 'on',
  labelUnchecked: 'off',
  colorChecked: 'green',
  colorUnchecked: '#aaa'
}
const px = v => v + 'px'

export default {
  name: 'toggleButton',
  props: {
    value: {
      type: Boolean,
      default: true
    },
    width: {
      type: Number,
      default: constants.width
    },
    height: {
      type: Number,
      default: constants.height
    },
    label: {
      type: [Boolean, Object],
      default: false,
      validator (value) {
        return typeof value === 'object' ? (value.checked || value.unchecked) : typeof value === 'boolean';
      }
    },
    color: {
      type: [Boolean, Object],
      default: false,
      validator (value) {
        return typeof value === 'object' ? (value.checked || value.unchecked) : typeof value === 'boolean';
      }
    },
  },
  data () {
    return {
      active: !!this.value,
    }
  },
  watch: {
    value (v) {
      this.active = !!v;
    }
  },
  computed: {
    labelStyle () {
      return {
        width: px(this.width),
        height: px(this.height),
        borderRadius: px(Math.round(this.height / 2)),
        backgroundColor: this.active ? this.colorChecked : this.colorUnchecked,
      }
    },
    switchLabelStyle () {
      return {
        lineHeight: px(this.height)
      }
    },
    colorChecked () {
      return this.color ? this.color.checked : constants.colorChecked;
    },
    colorUnchecked () {
      return this.color ? this.color.unchecked : constants.colorUnchecked;
    },
    labelChecked () {
      return this.label ? this.label.checked : constants.labelChecked;
    },
    labelUnchecked () {
      return this.label ? this.label.unchecked : constants.labelUnchecked;
    },
    moveLength () {
      return px(this.width - this.height + constants.margin);
    },
    marginLength () {
      return px(constants.margin);
    },
    circleStyle () {
      return {
        width: px(this.height-2*constants.margin),
        height: px(this.height-2*constants.margin),
        transition: `transform 300ms`,
        transform: this.active ? `translate3d(${this.moveLength}, ${this.marginLength}, 0)` 
          : `translate3d(${this.marginLength}, ${this.marginLength}, 0)`,
      }
    },
  },
  methods: {
    toggle (e) {
      this.active = !this.active;
      //register event
      this.$emit('input', this.active);
      this.$emit('change', {
        value: this.active,
        event: e
      });
    },
    
  }
}
</script>

<style lang="scss" scoped>
  .switch-input {
    display: none;
  }
  .switch-label {
    position: absolute;
    cursor: pointer;
    font-size: 10px;

    .switch-circle {
      border-radius: 100%;
      background: #fff;
      position: absolute;

    }

    .switch-label-c {
      position: absolute;
      color: white;
      font-weight: bold;

      &.switch-left {
        left: 6px;
      }
      &.switch-right {
        right: 6px;
      }
    }
  }

</style>

