<template>
  <transition name="slide">
    <div v-if="showTray" ref="trayRoot" class="checkbox-tray">
      <div
        class="prefecture-checkbox"
        v-for="pref in localPrefectures"
        :key="pref.code"
      >
        <input
          type="checkbox"
          :id="pref.code"
          :value="pref.code"
          :checked="isChecked(pref.code)"
          @change="toggleCheckbox(pref.code, $event.target.checked)"
        />
        <label :for="pref.code">{{ pref.name }}</label>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    showTray: Boolean,
    area: Object,
    selectedPrefCodes: Array,
    allPrefectures: Array,
    startIndex: Number,
  },
  computed: {
    localPrefectures() {
      return this.allPrefectures.slice(
        this.startIndex,
        this.startIndex + this.area.count
      );
    },
  },
  methods: {
    isChecked(code) {
      return this.selectedPrefCodes.includes(code);
    },
    toggleCheckbox(code, isChecked) {
      this.$emit("prefecture-toggled", code, isChecked);
    },
  },
};
</script>

<style>
.checkbox-tray {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  border-radius: 0px 0px 15px 15px;
  z-index: 0;
  position: relative;
}

.prefecture-checkbox {
  margin: 0rem 0.2rem 0rem 0.2rem;
  font-size: 20px;
  padding: 4px;
}

.slide-enter-active {
  transition: transform 0.3s ease-in-out, max-height 0.3s;
  max-height: 200px;
}

.slide-leave-active {
  transition: transform 0.3s ease-in-out, max-height 0.3s;
}
.slide-enter-from {
  max-height: 0px;
}

.slide-leave-to {
  max-height: 0px;
}

.slide-leave-from {
  max-height: 200px;
}
</style>
