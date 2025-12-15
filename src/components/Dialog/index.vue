<template>
  <transition name="dialog-fade">
    <div
      v-if="visible"
      class="dialog-overlay"
      @click="handleOverlayClick"
    >
      <transition name="dialog-zoom" appear>
        <div class="dialog-content" @click.stop>
          <div class="dialog-header">
            <h3>{{ title }}</h3>
            <button
              v-if="closable"
              class="close-btn"
              @click="close"
            >
              ×
            </button>
          </div>

          <div class="dialog-body">
            <slot />
          </div>

          <div class="dialog-footer" v-if="$slots.footer">
            <slot name="footer" />
          </div>
        </div>
      </transition>
    </div>
  </transition>
</template>


<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  visible: {
    type: Boolean,
    default: false
  },
  title: {
    type: String,
    default: ''
  },
  closable: {
    type: Boolean,
    default: true
  },
  maskClosable: {
    type: Boolean,
    default: true
  }
});

const emit = defineEmits(['update:visible', 'close']);

const close = () => {
  emit('update:visible', false);
  emit('close');
};

const handleOverlayClick = () => {
  if (props.maskClosable) {
    close();
  }
};
</script>

<style scoped>
.dialog-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.dialog-content {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  max-width: 90%;
  max-height: 90%;
  overflow: auto;
  min-width: 300px;
}

.dialog-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 24px;
  border-bottom: 1px solid #e8e8e8;
}

.dialog-header h3 {
  margin: 0;
  font-size: 16px;
  font-weight: 600;
  color: #333;
}

.close-btn {
  background: none;
  border: none;
  font-size: 24px;
  cursor: pointer;
  color: #999;
  padding: 0;
  width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.close-btn:hover {
  color: #666;
}

.dialog-body {
  padding: 24px;
  color: #666;
}

.dialog-footer {
  padding: 16px 24px;
  border-top: 1px solid #e8e8e8;
  display: flex;
  justify-content: flex-end;
  gap: 8px;
}

/* 动画效果 */
/* ========== 遮罩动画 ========== */
.dialog-fade-enter-active,
.dialog-fade-leave-active {
  transition: opacity 0.25s ease;
}

.dialog-fade-enter-from,
.dialog-fade-leave-to {
  opacity: 0;
}

/* ========== 弹窗动画 ========== */
.dialog-zoom-enter-active,
.dialog-zoom-leave-active {
  transition:
    opacity 0.25s ease,
    transform 0.25s cubic-bezier(0.4, 0, 0.2, 1);
  /* will-change: opacity, transform; */
}

.dialog-zoom-enter-from {
  opacity: 0;
  transform: scale(0.95) translateY(-10px);
}

.dialog-zoom-leave-to {
  opacity: 0;
  transform: scale(0.95) translateY(-10px);
}

</style>