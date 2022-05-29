<script>
export default {
  props: {
    position: {
      type: Number,
      required: true,
    },
    value: {
      type: String,
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
      <img :src="`/images/${value}.png`" :alt="value" />
      <!-- <img v-if="matched" src="/images/check.png" class="checkmark" /> -->
    </div>
    <div class="card-face is-back" v-else></div>
  </div>
</template>

<style>
img {
  height: 90px;
  width: 90px;
  background-size: cover;
}

.checkmark {
  position: absolute;
  height: 15px;
  width: 15px;
}
.card {
  border: 5px solid gray;
  position: relative;
}
.card-face {
  width: 100%;
  height: 100%;
  position: absolute;
  background: rgba(52, 235, 125, 0.1);
  background-size: cover;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
}
.card-face.is-front {
  background: rgba(52, 235, 125, 0.1);
  background-size: contain;
  color: white;
}
.card-face.is-back {
  background-image: url("../../public/images/save-the-world.png");
  background-size: cover;
  /* filter: opacity(0.5) drop-shadow(0 0 0 white); */
  background: rgba(1 1 1 0.25);
  color: white;
}
</style>
