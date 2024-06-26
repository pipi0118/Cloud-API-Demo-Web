<template>
  <a-popover :visible="state.sVisible" trigger="click" v-bind="$attrs" :overlay-class-name="overlayClassName"
    placement="bottom" @visibleChange=";" v-on="$attrs">
    <template #content>
      <div class="title-content">
      </div>
      <slot name="formContent" />
      <div class="uranus-popconfirm-btns">
        <a-button size="sm" @click="onCancel">
          {{ cancelText || 'cancel' }}
        </a-button>
        <a-button size="sm" :loading="loading" type="primary" class="confirm-btn" @click="onConfirm">
          {{ okText || 'ok' }}
        </a-button>
      </div>
    </template>
    <template v-if="$slots.default">
      <slot></slot>
    </template>
  </a-popover>
</template>

<script lang="ts" setup>
import { defineProps, defineEmits, reactive, watch, computed } from 'vue'

const props = defineProps<{
  visible?: boolean,
  loading?: Boolean,
  disabled?: Boolean,
  title?: String,
  okText?: String,
  cancelText?: String,
  width?: Number,
}>()

const emit = defineEmits(['cancel', 'confirm'])

const state = reactive({
  sVisible: false,
  loading: false,
})

watch(() => props.visible, (val) => {
  state.sVisible = val || false
})

const loading = computed(() => {
  return props.loading
})
const okLabel = computed(() => {
  return props.loading ? '' : '确定'
})

const overlayClassName = computed(() => {
  const classList = ['drone-control-popconfirm']
  return classList.join(' ')
})

function onConfirm (e: Event) {
  if (props.disabled) {
    return
  }
  emit('confirm', e)
}

function onCancel (e: Event) {
  state.sVisible = false
  emit('cancel', e)
}

</script>

<style lang="scss">
.drone-control-popconfirm {
  min-width: 300px;

  .uranus-popconfirm-btns {
    display: flex;
    padding: 10px 0px;
    justify-content: flex-end;

    .confirm-btn {
      margin-left: 10px;
    }
  }

  .form-content {
    display: flex;
    flex-direction: column;

    >div {
      display: flex;
      margin-bottom: 5px;

      .form-label {
        flex: 1 0 60px;
        margin-right: 10px;
      }
    }
  }
}
</style>
