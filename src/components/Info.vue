<template>
  <div class="component-container" :class="{ 'width-mobile': !ShowText }">
    <div :class="{ 'container-mobile': !ShowText }">
      <h1 :class="{ 'title-info-mobile': !ShowText }" class="title-info">
        {{ title }}
      </h1>
      <img
        :src="require('../assets/InfoVideo.png')"
        :alt="imageAlt"
        class="component-image"
        :class="{ 'component-image-mobile': !ShowText }"
      />
    </div>
    <div v-show="ShowText" style="display: flex; justify-content: center">
      <p class="text-info">{{ firstPart }}</p>
      <p class="last-word">{{ lastWord }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "InfoContainer",
  props: {
    title: {
      type: String,
      default: "Título predeterminado",
    },
    imageSrc: {
      type: String,
      default: null,
    },
    imageAlt: {
      type: String,
      default: "Descripción de la imagen",
    },
    text: {
      type: String,
      default: "Texto predeterminado",
    },
    ShowText: {
      type: Boolean,
      default: true,
    },
  },
  computed: {
    firstPart() {
      const words = this.text.split(" ");
      if (words.length > 1) {
        words.pop();
        return words.join(" ");
      } else {
        return "";
      }
    },
    lastWord() {
      const words = this.text.split(" ");
      if (words.length > 1) {
        return words[words.length - 1];
      } else {
        return this.text;
      }
    },
  },
};
</script>

<style>
.width-mobile {
  width: 85% !important;
}
.title-info {
  color: var(--primary-color);
  font-weight: bold;
  font-size: 300%;
}

.title-info-mobile {
  color: var(--primary-color);
  font-weight: bold;
  font-size: 130%;
  text-wrap: nowrap;
}

.text-info {
  font-size: 120%;
  color: gray;
}

.container-mobile {
  display: flex;
  flex-direction: row-reverse;
  align-items: center;
}

.component-image {
  margin-top: 15%;
  margin-bottom: 3%;
  width: 75%;
}

.component-image-mobile {
  margin-top: 0%;
  margin-bottom: 0%;
  width: 45%;
}

.last-word {
  font-size: 120%;
  color: var(--primary-color);
  margin-left: 8px;
  font-weight: 400;
}
</style>
