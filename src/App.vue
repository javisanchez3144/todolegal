<template>
  <div id="app">
    <MainNavbar />
    <div class="view-tabs hide-on-mobile">
      <TabsComponent :pageTitle="pageTitle" :tabs="tabs">
        <template v-slot="{ activeTab }">
          <div v-if="activeTab === 0">Contenido del Tab 1</div>
          <div v-if="activeTab === 1">Contenido del Tab 2</div>
          <div v-if="activeTab === 2">Contenido del Tab 3</div>
        </template>
      </TabsComponent>
    </div>
    <div class="cards-container">
      <div class="card card-left responsive-card">
        <div class="horizontal-row card-mobile">
          <div class="horizontal-col">
            <img src="./assets/compi.png" alt="Image" class="card-image" />
          </div>
          <div class="horizontal-col">
            <p class="card-title">¿Cómo Funciona?</p>
          </div>

        </div>
        <div class="card-laptop">
          <p class="card-title">¿Cómo Funciona?</p>
          <img src="./assets/compi.png" alt="Image" class="card-image" />
        </div>


        <p class="card-text card-description">Este es un servicio gratuito de <span
            class="blue-text"><b>todolegal</b></span></p>
      </div>
      <div class="card card-right">
        <p class="card-title-2">Carga de Documentos</p>
        <p class="subtitulo">Sube tus documentos y ordénalos <span class="question-icon" data-toggle="popover"
            data-content="Este es un mensaje de ayuda para el usuario.">
            <i class="fas fa-question-circle"></i>
          </span></p>
          <br>
        <div class="centered-input" v-if="uploadedFiles.length === 0">
          <div class="file-input backgroundFile" @click="clickFileInput">
            <label>
              <i class="fas fa-cloud-upload-alt"></i>
              <p class="text-input">Arrastra y suelta tus documentos aquí o busca archivo</p>
            </label>
            <input ref="fileInput" type="file" id="fileInput" @change="handleFileChange" multiple accept=".pdf"
              style="display: none;">
          </div>
        </div>
        <br><br>
        <div class="uploaded-files" v-if="uploadedFiles.length > 0">
          <div class="file-input-2 backgroundFile">
            <div v-for="(dir, key) in uploadedFiles" :key="key" class="horizontal-row bordered-box">
              <div class="horizontal-col">
                <i class="fas fa-bars"></i>
                <span class="vertical-bar"></span>
              </div>
              <div class="horizontal-col filename-col">
                {{ truncateText(dir.name, 20) }}
                <span class="vertical-bar delete-col"></span>
              </div>
              <div class="horizontal-col delete-col" @click="eliminarArchivo(key)">
                <i class="fas fa-trash"></i>
              </div>
            </div>
            <div style="color: blueviolet;" class="horizontal-row bordered-box backgroundFile"
              v-if="uploadedFiles.length > 0">
              <div class="horizontal-col">
                <i class="fas fa-plus"></i>
                <span class="vertical-bar"></span>
              </div>
              <div class="horizontal-col filename-col" @click="clickFileInput">
                Añadir más documentos
                <span class="vertical-bar delete-col"></span>
              </div>
              <input ref="fileInput" type="file" id="fileInput" @change="handleFileChange" multiple accept=".pdf"
                style="display: none;">
              <div class="horizontal-col delete-col">
                <p class="text-anadir">5 Máx <br> 20 Mb</p>
              </div>
            </div>
          </div>

        </div>
        <br>
        <button class="next-button" @click="registrar">Siguiente</button>
        <br>
      </div>
    </div>
  </div>
</template>

<script>
import MainNavbar from "./components/SiteNavbar.vue";
import TabsComponent from "@/components/TabsComponent.vue";
import '@fortawesome/fontawesome-free/css/all.css';
import axios from 'axios';
export default {
  name: 'App',
  components: {
    MainNavbar,
    TabsComponent
  },
  data() {
    return {
      pageTitle: "Firmar nuevo documento",
      tabs: [
        { number: 1, title: "Cargar documento" },
        { number: 2, title: "Indicar firmantes" },
        { number: 3, title: "Personalizaciones opcionales" },
      ],
      uploadedFiles: [],
      validarArchivos: true
    };
  },
  methods: {
    truncateText(text, maxLength) {
      if (text.length > maxLength) {
        return text.slice(0, maxLength - 3) + '...';
      }
      return text;
    },
    handleFileChange(event) {
      const files = event.target.files;
      for (let i = 0; i < files.length; i++) {
        this.uploadedFiles.push(files[i]);
      }
      this.validarArchivos = false
      this.$refs.fileInput.value = '';
    },
    clickFileInput() {
      this.$refs.fileInput.click();
    },
    removeFile(index) {
      this.uploadedFiles.splice(index, 1);
    },
    addMoreFiles() {
      this.$nextTick(() => {
        this.$refs.fileInput;
      });
    },
    eliminarArchivo(key) {
      this.uploadedFiles.splice(key, 1)
    },
    async getPublicIP() {
      try {
        const response = await fetch('https://api.ipify.org?format=json');
        const data = await response.json();
        return data.ip;
      } catch (error) {
        console.error('Error al obtener la dirección IP:', error);
        return null;
      }
    },
    async registrar() {
      try {
        const publicIP = await this.getPublicIP();
        const localDate = new Date();
        const formattedLocalDate = localDate.toISOString();
        const response = await axios.post('https://webhook.site/04b12921-3072-4fe9-8d60-9f4394e109d9', {
          name: 'Javier Sanchez',
          localDate: formattedLocalDate,
          ip: publicIP
        });

        console.log('Respuesta del servidor:', response.data);
      } catch (error) {
        console.error('Error en la petición POST:', error);
      }
    }
  },
}
</script>

<style>
@import "./assets/css/App.css";
</style>
