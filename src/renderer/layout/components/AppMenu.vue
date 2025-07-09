<template>
  <div>
    <!-- menu -->
    <div class="app-menu" :class="{ 'app-menu-expanded': isText }">
      <div class="app-menu-header">
        <div class="app-menu-logo" @click="isText = !isText">
          <!-- 新的音乐主题图标 -->
          <div class="modern-music-logo">
            <div class="music-note">
              <div class="note-head"></div>
              <div class="note-stem"></div>
              <div class="note-flag"></div>
            </div>
            <div class="sound-waves">
              <div class="wave wave-1"></div>
              <div class="wave wave-2"></div>
              <div class="wave wave-3"></div>
            </div>
          </div>
        </div>
      </div>
      <div class="app-menu-list">
        <div v-for="(item, index) in menus" :key="item.path" class="app-menu-item">
          <n-tooltip :delay="200" :disabled="isText || isMobile" placement="bottom">
            <template #trigger>
              <router-link class="app-menu-item-link" :to="item.path">
                <i class="iconfont app-menu-item-icon" :style="iconStyle(index)" :class="item.meta.icon"></i>
                <span v-if="isText" class="app-menu-item-text ml-3" :class="isChecked(index) ? 'text-primary' : ''">{{ t(item.meta.title) }}</span>
              </router-link>
            </template>
            <div v-if="!isText">{{ t(item.meta.title) }}</div>
          </n-tooltip>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { useRoute } from 'vue-router';
import { ref, watch } from 'vue';
import { useI18n } from 'vue-i18n';

import { isMobile } from '@/utils';

const props = defineProps({
  size: {
    type: String,
    default: '26px'
  },
  color: {
    type: String,
    default: '#aaa'
  },
  selectColor: {
    type: String,
    default: '#00d4aa'
  },
  menus: {
    type: Array as any,
    default: () => []
  }
});

const route = useRoute();
const path = ref(route.path);
watch(
  () => route.path,
  async (newParams) => {
    path.value = newParams;
  }
);

const { t } = useI18n();

const isChecked = (index: number) => {
  return path.value === props.menus[index].path;
};

const iconStyle = (index: number) => {
  const style = {
    fontSize: props.size,
    color: isChecked(index) ? props.selectColor : props.color
  };
  return style;
};

const isText = ref(false);
</script>

<style lang="scss" scoped>
.app-menu {
  @apply flex-col items-center justify-center transition-all duration-300 w-[100px] px-1;
}

.app-menu-expanded {
  @apply w-[160px];

  .app-menu-item {
    @apply hover:bg-primary-50 dark:hover:bg-primary-900/20 rounded-lg mr-4 transition-all duration-200;
  }
}

.app-menu-item-link,
.app-menu-header {
  @apply flex items-center w-[200px] overflow-hidden ml-2 px-5;
}

.app-menu-header {
  @apply ml-1;
}

.app-menu-item-link {
  @apply mb-6 mt-6;
}

.app-menu-item-icon {
  @apply transition-all duration-300 text-neutral-500 dark:text-neutral-400;
  filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.1));

  &:hover {
    color: #00d4aa !important;
    transform: scale(1.1) rotate(5deg);
    filter: drop-shadow(0 4px 8px rgba(0, 212, 170, 0.4));
  }
}

.mobile {
  .app-menu {
    max-width: 100%;
    width: 100vw;
    position: fixed;
    bottom: 0;
    left: 0;
    z-index: 99999;
    @apply bg-white/90 dark:bg-dark-100/90 backdrop-blur-md border-t border-primary-200 dark:border-primary-700;

    &-header {
      display: none;
    }

    &-list {
      @apply flex justify-between px-4;
    }

    &-item {
      &-link {
        @apply my-2 w-auto px-2;
        width: auto !important;
        margin-top: 8px;
        margin-bottom: 8px;
      }
    }

    &-expanded {
      @apply w-full;
    }
  }
}

/* 现代音乐Logo样式 */
.modern-music-logo {
  @apply w-9 h-9 relative flex items-center justify-center cursor-pointer;
  transition: all 0.3s ease;
  
  &:hover {
    transform: scale(1.1) rotate(5deg);
    filter: drop-shadow(0 4px 12px rgba(0, 212, 170, 0.5));
  }
}

.music-note {
  @apply relative;
  width: 20px;
  height: 24px;
  
  .note-head {
    @apply absolute bottom-0 left-0;
    width: 8px;
    height: 6px;
    background: linear-gradient(135deg, #00d4aa, #33e2c0);
    border-radius: 50%;
    transform: rotate(-20deg);
    box-shadow: 0 2px 4px rgba(0, 212, 170, 0.3);
  }
  
  .note-stem {
    @apply absolute;
    width: 2px;
    height: 16px;
    background: linear-gradient(180deg, #00d4aa, #00a688);
    left: 6px;
    bottom: 4px;
    border-radius: 1px;
  }
  
  .note-flag {
    @apply absolute;
    width: 8px;
    height: 8px;
    background: linear-gradient(135deg, #6366f1, #818cf8);
    right: 0;
    top: 2px;
    border-radius: 0 4px 4px 0;
    clip-path: polygon(0 0, 100% 0, 80% 100%, 0 60%);
  }
}

.sound-waves {
  @apply absolute -right-1 top-1/2;
  transform: translateY(-50%);
  
  .wave {
    @apply absolute;
    width: 2px;
    background: linear-gradient(180deg, #f59e0b, #fbbf24);
    border-radius: 1px;
    animation: wave-pulse 1.5s ease-in-out infinite;
    
    &.wave-1 {
      height: 4px;
      right: 0;
      animation-delay: 0s;
    }
    
    &.wave-2 {
      height: 8px;
      right: 4px;
      animation-delay: 0.2s;
    }
    
    &.wave-3 {
      height: 6px;
      right: 8px;
      animation-delay: 0.4s;
    }
  }
}

@keyframes wave-pulse {
  0%, 100% {
    transform: scaleY(0.5);
    opacity: 0.6;
  }
  50% {
    transform: scaleY(1);
    opacity: 1;
  }
}
</style>
