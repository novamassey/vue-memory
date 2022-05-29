<script>
export default {
  props: {
    position: {
      type: Number,
      required: true,
    },
    value: {
      type: Number,
      required: true,
    },
    visible: { type: Boolean, default: false },
    matched: { type: Boolean, default: false },
  },
  setup(props, context) {
    const selectCard = () => {
      context.emit("select-card", {
        position: props.position,
        faceValue: props.value,
      });
    };
    return {
      selectCard,
    };
  },
};
</script>

<template>
  <div class="card" @click="selectCard">
    <div class="card-face is-front" v-if="visible">
      {{ value }}-{{ matched }}
    </div>
    <div class="card-face is-back" v-else>Back</div>
  </div>
</template>

<style>
.card {
  border: 5px solid gray;
  position: relative;
}
.card-face {
  width: 100%;
  height: 100%;
  position: absolute;
}
.card-face.is-front {
  background-color: red;
  color: white;
}
.card-face.is-back {
  background-color: blue;
  color: white;
}
</style>
