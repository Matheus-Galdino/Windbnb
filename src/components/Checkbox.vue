<template>
  <div class="checkbox-container">
    <div class="checkbox" @click="changeChecked" :style="getStyles">
      <span class="material-icons" v-show="checked">done</span>
    </div>
    <p v-show="text != ''" @click="changeChecked">{{ text }}</p>
  </div>
</template>

<script>
export default {
  name: "Checkbox",
  props: {
    text: {
      type: String,
    },
    backgroundColor: {
      type: String,
      default: "#fff",
    },
    color: {
      type: String,
      default: "#5656EB",
    },
  },
  data() {
    return {
      checked: false,
    };
  },
  methods: {
    changeChecked() {
      this.checked = !this.checked;
      this.$emit("change", this.checked);
    },
  },
  computed: {
    getStyles() {
      return this.checked
        ? `background-color: ${this.backgroundColor};color: ${this.color};border:none;`
        : "";
    },
  },
};
</script>

<style lang="scss" scoped>
.checkbox-container {
  display: flex;
  align-items: center;
  column-gap: 0.7rem;

  * {
    cursor: pointer;
  }

  p {
    color: #4f4f4f;
    font-size: 1.3rem;
  }
}

.checkbox {
  width: 22px;
  height: 22px;

  display: flex;
  align-items: center;
  justify-content: center;

  border-radius: 4px;
  border: 1px solid #242830;
  transition: all 0.4s ease-out;

  span {
    font-size: 2rem;
  }
}
</style>