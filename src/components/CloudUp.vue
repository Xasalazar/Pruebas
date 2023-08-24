<template>
  <div class="component-container" :class="{ 'width-mobile': !ShowText }">
    <div class="title-container">
      <svg
        v-show="selectedFiles.length > 0 && ShowText"
        class="chev-icon"
        viewBox="0 0 24 24"
        :width="30"
        :height="30"
        @click="exitFile"
      >
        <path :fill="chevColor" :d="iconChev" />
      </svg>
      <p class="title-drop">{{ title }}</p>
    </div>
    <div class="subtitle-container">
      <p class="subtitle-drop" :class="{ 'icon-subtitle-mobile': !ShowText }">
        {{ subtitle }}
      </p>
      <img
        class="help-icon"
        :class="{ 'icon-subtitle-mobile': !ShowText }"
        src="../assets/help.png"
        alt="Help"
      />
    </div>

    <div style="display: flex; justify-content: center">
      <div
        @dragenter.prevent="toggleActive"
        @dragleave.prevent="toggleActive"
        @dragover.prevent
        @drop.prevent="handleFileDrop"
        :class="{
          'active-dropzone': active,
          'dropzone-width-mobile': !ShowText && selectedFiles.length > 0,
        }"
        class="dropzone"
      >
        <div v-show="selectedFiles.length == 0 || !ShowText">
          <label for="dropzoneFile">
            <img src="../assets/FileUploader.png" alt="Subida de Archivos" />
          </label>
          <label v-show="ShowText" class="label-drop" for="dropzoneFile"
            >Arrastra o suelta tus documentos aquí o
            <p :class="{ 'active-archive': active }" class="archive-drop">
              Buscar Archivo
            </p>
          </label>
        </div>
        <input
          class="input-drop"
          type="file"
          ref="fileInput"
          id="dropzoneFile"
          @change="fileAdded"
          accept=".pdf"
          multiple
        />
        <div v-show="selectedFiles.length > 0" class="file-list">
          <div class="file-grid">
            <div
              v-for="(file, index) in selectedFiles"
              :key="index"
              class="file-item"
            >
              <svg
                class="icon-item"
                viewBox="0 0 24 24"
                :width="30"
                :height="30"
              >
                <path :fill="iconcolor" :d="iconClass" />
              </svg>
              <p class="file-name">{{ file.name }}</p>
              <button class="remove-button" @click="removeFile(index)">
                <img src="../assets/trash.png" alt="Eliminacion de Archivos" />
              </button>
            </div>
            <div class="file-item-more" v-show="selectedFiles.length < 5">
              <svg viewBox="0 0 24 24" :width="30" :height="30">
                <path :fill="iconcolor" :d="iconPlus" />
              </svg>
              <label
                for="dropzoneFile"
                class="file-name"
                style="color: #007bff"
              >
                Añadir más documentos
              </label>
              <div style="white-space: nowrap">
                <p class="max-message">5 Máx</p>
                <p class="max-message">20 Mb</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <label v-show="selectedFiles.length == 0" class="secure-label"
      >Solo se permite archivos en formato PDF</label
    >

    <div v-show="ShowText" class="submit-button-container">
      <button class="submit-button-cloud" @click="send()">Siguiente</button>
    </div>
  </div>
  <loading :active="loading" :can-cancel="false"></loading>
</template>

<script>
import { ref } from "vue";
import { mdiMenu, mdiPlus, mdiChevronLeft } from "@mdi/js";
import { useToast } from "vue-toastification";
import Loading from "vue-loading-overlay";
import "vue-loading-overlay/dist/css/index.css";
import axios from "axios";
export default {
  name: "FileUploader",
  components: { Loading },

  setup(_, { emit }) {
    const toast = useToast();
    const active = ref(false);
    const loading = ref(false);
    const toggleActive = () => {
      active.value = !active.value;
    };

    const selectedFiles = ref([]);
    const maxFileNameLength = 40;
    const maxFileSize = 20 * 1024 * 1024;
    const webhook_KEY = "fcb7edfb-7686-4849-b411-feb1577ce0a7";
    const webApiIp_KEY = "9713d3c4bea543658af046a491698c4c";
    const fileAdded = (event) => {
      let files = null;
      if (event.target) {
        files = event.target.files;
      } else {
        files = event;
      }
      if (files && files.length > 0 && selectedFiles.value.length < 5) {
        for (const file of files) {
          if (!file.name.toLowerCase().endsWith(".pdf")) {
            toast.error("El archivo que intenta subir no es PDF", {
              position: "bottom-right",
            });
            return;
          }
          if (file.name.length > maxFileNameLength) {
            toast.error("El nombre del archivo es muy largo", {
              position: "bottom-right",
            });
            return;
          }
          if (file.size > maxFileSize) {
            toast.error("El archivo que intenta subir es muy pesado", {
              position: "bottom-right",
            });
            return;
          }
          selectedFiles.value.push(file);
          emit("BottomFile", selectedFiles.value.length > 0);
        }
      }
    };
    const removeFile = (index) => {
      selectedFiles.value.splice(index, 1);
      emit("BottomFile", selectedFiles.value.length > 0);
    };

    const exitFile = () => {
      selectedFiles.value = [];
    };

    const sendWebSiteHook = (dato) => {
      axios
        .post("https://webhook.site/" + webhook_KEY, dato, {
          headers: {
            "Content-Type": "multipart/form-data",
          },
        })
        .then((response) => {
          console.log("Respuesta de la API:", response.data);
        })
        .catch((error) => {
          console.error("Error al enviar datos a la API:", error);
        });

      selectedFiles.value = [];
    };
    const send = () => {
      const dato = selectedFiles.value;
      loading.value = true;
      if (dato.length === 0) {
        toast.error("No ha subido ningun archivo", {
          position: "bottom-right",
        });
        loading.value = false;
        return;
      }

      axios
        .get(
          "https://ipgeolocation.abstractapi.com/v1/?api_key=" + webApiIp_KEY
        )
        .then((response) => {
          console.log("Respuesta de la API:", response.data);
          const MyIp = response.data.ip_address;
          dato.push("Xavier Salazar");
          dato.push(new Date().toLocaleTimeString());
          dato.push(MyIp);
          sendWebSiteHook(dato);
        })
        .catch((error) => {
          console.error("Error al obtener la dirección IP:", error);
          toast.error("Error al obtener IP", {
            position: "bottom-right",
          });
        })
        .finally(() => {
          loading.value = false;
        });
    };

    return {
      active,
      toggleActive,
      selectedFiles,
      toast,
      fileAdded,
      removeFile,
      exitFile,
      send,
      sendWebSiteHook,
      loading,
    };
  },

  props: {
    title: {
      type: String,
      default: "Título predeterminado",
    },
    subtitle: {
      type: String,
      default: "Subtítulo predeterminado",
    },
    ShowText: {
      type: Boolean,
      default: true,
    },
  },
  computed: {
    iconClass() {
      return mdiMenu;
    },
    iconPlus() {
      return mdiPlus;
    },
    iconChev() {
      return mdiChevronLeft;
    },
    iconcolor() {
      return "#007bff";
    },
    chevColor() {
      return "#002179";
    },
  },
  methods: {
    handleFileDrop(event) {
      this.toggleActive();
      if (event.dataTransfer && event.dataTransfer.files) {
        const files = event.dataTransfer.files;
        this.fileAdded(files);
      }
    },
    openFileInput() {
      // Simula un clic en el elemento input oculto
      this.$refs.fileInput.click();
    },
  },
};
</script>

<style>
.width-mobile {
  width: 85%;
}

.dropzone-width-mobile {
  width: 90% !important;
}
.icon-subtitle-mobile {
  margin-bottom: 20px !important;
}
.help-icon {
  margin-left: 5px;
  margin-top: 7px;
  margin-bottom: 65px;
}
.chev-icon {
  margin-top: 5px;
}
.subtitle-container {
  display: flex;
  align-items: flex-start;
}
.title-container {
  display: flex;
}
.max-message {
  margin: 0%;
  color: var(--primary-color);
  font-weight: bold;
}

.file-message {
  width: 450px;
  display: flex;
  align-items: center;
  background-color: #f4f6fc;
  border: 1px solid var(--secondary-color);
  padding: 10px;
  border-radius: 20px;
}

.title-drop {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  margin-top: 5px;
  margin-bottom: 0px;
  font-size: larger;
  font-weight: bold;
  color: var(--tertiary-color);
}

.subtitle-drop {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  margin-top: 8px;
  margin-bottom: 65px;
  font-size: small;
}
.secure-label {
  color: red;
  font-size: small;
}
.archive-drop {
  color: var(--primary-color);
  margin-left: 5px;
}
.label-drop {
  font-size: small;
  display: flex;
  align-items: center;
}

.active-dropzone {
  background-color: var(--primary-color) !important;
  color: var(--secondary-color);
}

.active-archive {
  color: var(--secondary-color);
}
.input-drop {
  display: none;
}
.submit-button-container {
  display: flex;
  justify-content: center;
  margin-top: 25%;
}

.submit-button-cloud {
  background-color: var(--primary-color);
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 10px;
  cursor: pointer;
  width: 80%;
  height: 60px;
  max-width: 300px;
  text-align: center;
  font-size: 16px;
  transition: background-color 0.3s ease;
}

.submit-button:hover {
  background-color: #0056b3;
}

.remove-button {
  background: none;
  border: none;
  cursor: pointer;
}

.file-item {
  margin: 10px;
  display: flex;
  align-items: center;
  background-color: var(--secondary-color);
  border: 1px #ccc;
  padding: 10px;
  border-radius: 20px;
}

.file-item-more {
  margin: 10px;
  display: flex;
  align-items: center;
  background-color: #f4f6fc;
  border: 3px solid var(--secondary-color);
  padding: 10px;
  border-radius: 20px;
}

.file-name {
  text-align: initial;
  flex-grow: 1;
  margin: 0 10px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  color: #8e8e8e;
}

.dropzone {
  padding: 5%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  row-gap: 16px;
  border: 2px dashed var(--primary-color);
  background-color: #f4f6fc;
  border-radius: 20px;
  margin-bottom: 3%;
  width: 70%;
}
.file-list {
  display: flex;
  flex-direction: column; /* Cambiado de row a column */
  align-items: center;
  width: 95%;
}

.file-grid {
  width: 100%;
}

.icon-item {
  overflow-clip-margin: content-box;
  overflow: initial;
}
</style>
