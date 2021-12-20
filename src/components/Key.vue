<template>
  <div :class="['keySquare', pressed ? 'pressed' : '']">{{ code }}</div>
</template>

<script>
export default {
  props: {
    code: {
      type: String,
      required: true,
    },
    pressed: {
      type: Boolean,
      required: true,
    },
  },
};
</script>

<style scoped>
.keySquare {
  border: 1px solid var(--bright_blue);
  padding: 6px 12px;
  border-radius: 5px;
  color: var(--bright_blue);
  margin-right: 5px;
  font-weight: bold;
  user-select: none;
}

.keySquare.pressed {
  background-color: var(--neutral_purple);
  border: 1px solid var(--neutral_purple);
  color: var(--dark0_hard);
}
</style>
