<script>
import LayoutInitial from "@/layouts/LayoutInitial.vue";
import StepByStepForm from "@/components/Steps.vue";
import InfoContainer from "@/components/Info.vue";
import CloudUploader from "@/components/CloudUp.vue";

export default {
  name: "DocumentsUp",
  created() {
    window.addEventListener("resize", this.handleResize);
    window.addEventListener("orientationchange", this.handleResize);
    this.handleResize();
  },
  data() {
    return {
      isMobile: false,
      isSemiMobile: false,
      boolBottom: false,
    };
  },
  beforeUnmount() {
    window.removeEventListener("resize", this.handleResize);
    window.removeEventListener("orientationchange", this.handleResize);
  },
  components: {
    LayoutInitial,
    StepByStepForm,
    InfoContainer,
    CloudUploader,
  },
  props: {
    msg: String,
  },
  methods: {
    handleResize() {
      this.isMobile = window.innerWidth <= 800;
      this.isSemiMobile = window.innerWidth <= 1300;
    },
    handleFileAdded(logic) {
      this.boolBottom = logic;
    },
    EliminarArchivos() {
      this.$refs.CloudUploaderRef.removeFile();
    },
    EnviarArchivos() {
      this.$refs.CloudUploaderRef.send();
    },
  },
};
</script>

<template>
  <LayoutInitial :ShowText="!isMobile">
    <StepByStepForm
      v-show="!isMobile"
      :ShowText="!isSemiMobile"
      title="Firmar nuevo documento"
      :steps="[
        'Cargar Documentos',
        'Indicar Firmantes',
        'Personalizaciones Opcionales',
      ]"
    />
    <div
      :class="{ 'box-container': !isMobile, 'box-container-mobile': isMobile }"
    >
      <InfoContainer
        title="¿Cómo funciona?"
        text="Este es un servicio gratuito de todolegal"
        imageSrc="/assets/InfoVideo.png"
        :ShowText="!isMobile"
      ></InfoContainer>
      <CloudUploader
        ref="CloudUploaderRef"
        title="Carga de Documentos"
        subtitle="Sube tus documentos y ordènalos"
        :ShowText="!isMobile"
        @BottomFile="handleFileAdded"
      ></CloudUploader>
    </div>
  </LayoutInitial>
  <div v-show="isMobile" class="submit-container-mobile">
    <button
      v-show="boolBottom"
      class="submit-button-prev"
      @click="EliminarArchivos()"
    >
      Regresar
    </button>
    <button class="submit-button-next" @click="EnviarArchivos()">
      Siguiente
    </button>
  </div>
</template>

<style>
.box-container {
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: stretch;
  margin-top: 1%;
  max-width: 82%;
}

.box-container-mobile {
  margin: 0 auto;
  width: 100%;
}
.submit-button-prev {
  background-color: var(--secondary-color);
  color: var(--primary-color);
  border: solid var(--primary-color);
  padding: 10px 20px;
  cursor: pointer;
  width: 100%;
  height: 60px;
  text-align: center;
  font-size: 16px;
}

.submit-button-next {
  background-color: var(--primary-color);
  color: var(--secondary-color);
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  width: 100%;
  height: 60px;
  text-align: center;
  font-size: 16px;
}
.submit-container-mobile {
  position: fixed;
  bottom: 0;
  width: 100%;
  display: flex;
}
</style>
