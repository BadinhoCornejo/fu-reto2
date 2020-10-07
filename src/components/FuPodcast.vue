<template>
  <div :class="['podcast', { 'podcast--active': active }]">
    <img class="podcast__image" :src="image" :alt="name" />
    <p class="podcast__name">{{ name }}</p>
  </div>
</template>

<script>
export default {
  name: "FuPodcast",
  props: {
    image: {
      type: String,
      required: true,
    },
    name: {
      type: String,
      required: true,
    },
    active: {
      type: Boolean,
      default: false,
      required: false,
    },
  },
};
</script>

<style scoped>
.podcast {
  width: 145px;
  text-align: center;
  cursor: pointer;
}

.podcast--active {
  border: 1px solid blue;
}

.podcast__image {
  width: 144px;
  height: 164px;
  object-fit: cover;
}

.podcast__name {
  margin: 0;
  margin-top: 4px;
  font-size: 14px;
  font-weight: bold;
}
</style>