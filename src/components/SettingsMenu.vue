<template>
  <transition name="fade">
    <div
      v-show="menuOpen"
      id="settings-menu"
      ref="menu"
      class="menu"
    >
      <SettingsMenuLanguage />
      <SettingsMenuGraphics
        :graphics="graphics"
        @change="onGraphicsChange"
      />
      <SettingsMenuPoints
        :points="points"
        @change="onPointsChange"
      />
    </div>
  </transition>
</template>

<script>
import SettingsMenuGraphics from './SettingsMenuGraphics.vue';
import SettingsMenuPoints from './SettingsMenuPoints.vue';
import SettingsMenuLanguage from './SettingsMenuLanguage.vue';

export default {
  name: 'SettingsMenu',
  components: {
    SettingsMenuGraphics,
    SettingsMenuPoints,
    SettingsMenuLanguage,
  },
  props: {
    graphics: {
      type: String,
      required: true,
      validator(value) {
        return ['low', 'medium', 'high'].includes(value);
      },
    },
    points: {
      type: Number,
      required: true,
      validator(value) {
        return value >= 200000 && value <= 10000000;
      },
    },
  },
  data() {
    return {
      menuOpen: false,
    };
  },
  created() {
    document.addEventListener('click', this.documentClick);
  },
  destroyed() {
    document.removeEventListener('click', this.documentClick);
  },
  methods: {
    toggleMenu() {
      this.menuOpen = !this.menuOpen;
    },
    onGraphicsChange(graphics) {
      this.$emit('graphics-change', graphics);
    },
    onPointsChange(points) {
      this.$emit('points-change', points);
    },
    documentClick(e) {
      if (
        !this.$el.parentElement.contains(e.target)
        && !e.target.classList.contains('shepherd-button')
        && !e.target.classList.contains('shepherd-modal-target')
      ) {
        if (this.menuOpen) this.menuOpen = false;
      }
    },
  },
};
</script>

<style scoped>
#settings-menu >>> h4 {
  font-weight: normal;
}

.menu {
  border-radius: 10px;
  background-color: #454545;
  padding: 1rem;
  color: #ffffff;
  margin-left: 20px;
  transform: translateY(2px);
  width: 12rem;
  z-index: 1;
}

.menu:before {
  content: '';
  width: 0px;
  height: 0px;
  position: absolute;
  border-left: 10px solid transparent;
  border-right: 10px solid #454545;
  border-top: 10px solid transparent;
  border-bottom: 10px solid transparent;
  left: -20px;
}

.menu:focus {
  outline: 0;
}

.menu-item {
  margin-bottom: 1.6rem;
}
</style>
