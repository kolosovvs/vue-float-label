<template>
  <div class="vfl-has-label">
    <label class="vfl-label" :class="classObject" :for="inputId">
      {{ floatLabel }}
    </label>
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'float-label',
  props: {
    on: {
      type: Boolean,
      default: true
    },
    fixed: {
      type: Boolean,
      default: false
    },
    label: {
      type: String,
      default: ''
    },
    dispatch: {
      type: Boolean,
      default: true
    },
    for: {
      type: String,
      default: null
    }
  },
  data () {
    return {
      formEl: undefined,
      labelEl: undefined,
      isActive: false,
      isFocused: false,
      labelText: ''
    }
  },
  mounted () {
    this.formEl = this.$el.querySelector('input, textarea, select')
    this.formEl.addEventListener('input', this.updateIsActive)
    this.formEl.addEventListener('input', this.updateIsFocused)
    this.formEl.addEventListener('blur', this.updateIsFocused)
    this.formEl.addEventListener('focus', this.updateIsFocused)

    if (!this.for) {
      this.labelEl = this.$el.querySelector('label')
      this.labelEl.addEventListener('click', this.focusFormEl)
    }

    // eslint-disable-next-line no-undef
    this.observer = new MutationObserver(mutations => {
      for (const m of mutations) {
        const newValue = m.target.getAttribute(m.attributeName)
        this.$nextTick(() => {
          this.labelText = newValue
        })
      }
    })
    this.observer.observe(this.formEl, {
      attributes: true,
      attributeOldValue: true,
      attributeFilter: ['placeholder']
    })

    this.dispatchInput()
  },
  beforeDestroy () {
    this.formEl.removeEventListener('input', this.updateIsActive)
    this.formEl.removeEventListener('input', this.updateIsFocused)
    this.formEl.removeEventListener('blur', this.updateIsFocused)
    this.formEl.removeEventListener('focus', this.updateIsFocused)
    this.observer.disconnect()
  },
  methods: {
    dispatchInput () {
      if (this.dispatch) {
        const event = document.createEvent('HTMLEvents')
        event.initEvent('input', true, false)
        this.formEl.dispatchEvent(event)
      }
    },
    focusFormEl () {
      this.formEl.focus()
    },
    updateIsActive (e) {
      return e.target === document.activeElement
    },
    updateIsFocused (e) {
      if (this.formElType === 'input' || this.formElType === 'textarea') {
        this.labelText = this.formEl.placeholder
      }
      this.isFocused = e.target === document.activeElement
      this.isActive = this.isFocused || e.target.value
    }
  },
  computed: {
    inputId () {
      return this.for
    },
    classObject () {
      return {
        'vfl-label-on-input': this.on && (this.isActive || this.fixed),
        'vfl-label-on-focus': this.isFocused
      }
    },
    formElType () {
      return this.formEl ? this.formEl.tagName.toLowerCase() : ''
    },
    floatLabel () {
      if (this.labelText) return this.labelText
      if (this.label) return this.label

      switch (this.formElType) {
        case 'input':
          return this.formEl.placeholder
        case 'textarea':
          return this.formEl.placeholder
        case 'select':
          return this.formEl.querySelector('option[disabled][selected]').innerHTML
        default:
          return ''
      }
    }
  }
}
</script>

<style>
.vfl-has-label {
  position: relative;
}

.vfl-label {
  position: absolute;
  top: 0;
  left: 25px;
  overflow: hidden;
  font-family: sans-serif;
  font-size: 0.8em;
  color: #aaa;
  text-overflow: ellipsis;
  white-space: nowrap;
  pointer-events: none;
  opacity: 0;
  transition: all 0.2s ease-out;
}

.vfl-label-on-input {
  top: -1.3em;
  pointer-events: all;
  opacity: 1;
}

.vfl-label-on-focus {
  color: #0074d9;
}

.vfl-has-label input:focus,
.vfl-has-label textarea:focus,
.vfl-has-label select:focus {
  outline: 0;
}
</style>
