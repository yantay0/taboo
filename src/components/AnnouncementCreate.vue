<template>
  <div class="container">
    <div class="row">
      <div class="col-xl-6">
        <div class="create-lost-pet">
          <h2 style="margin-top: 25px; font-weight: bold; margin-bottom: 25px;">Основная информация</h2>
          <div class="create_details">
            <!-- Что произошло с питомцем? -->
            <div class="form-group">
              <label for="typeAnnouncement">Что произошло с питомцем?</label>
              <select id="typeAnnouncement" class="form-select" v-model="formData.type">
                <option disabled value="">Выберите тип объявления</option>
                <option v-for="item in typeAnnouncement" :key="item.id" :value="item.type">
                  {{ item.name }}
                </option>
              </select>
            </div>

            <!-- Имя питомца? -->
            <div class="form-group">
              <label for="animalName">Имя питомца?</label>
              <textarea id="animalName" class="form-control" style="height: 17px;" v-model="formData.animal.animalName"></textarea>
            </div>
          </div>

          <div class="create_details">
            <!-- Вид питомца? -->
            <div class="form-group">
              <label for="animalType">Вид питомца?</label>
              <select id="animalType" class="form-select" v-model="formData.animal.type" @change="onAnimalTypeChange">
                <option disabled value="">Выберите вид животного</option>
                <option v-for="item in animalType" :key="item.id" :value="item.name">
                  {{ item.name }}
                </option>
              </select>
            </div>
            <!-- Цвет животного? -->
            <div class="form-group">
              <label for="animalColor">Цвет животного?</label>
              <select id="animalColor" class="form-select" v-model="formData.animal.color">
                <option disabled value="">Выберите цвет животного</option>
                <option v-for="item in animalColors" :key="item.id" :value="item.name">
                  {{ item.name }}
                </option>
              </select>
            </div>
          </div>

          <!-- Порода питомца? -->
          <div class="form-group" v-if="formData.animal.type !== ''">
            <label for="animalBreed">Порода питомца?</label>
            <select id="animalBreed" class="form-select" v-model="formData.animal.breed">
              <option disabled value="">Выберите породу животного</option>
              <option v-for="item in animalBreeds" :key="item.id" :value="item.name">
                {{ item.name }}
              </option>
            </select>
          </div>

          <!-- Подробности -->
          <div class="form-group">
            <label for="details">Подробности</label>
            <textarea id="details" class="form-control" v-model="formData.details"></textarea>
          </div>

          <h2 style="margin-top: 25px; font-weight: bold; margin-bottom: 10px;">Место пропажи\Находки</h2>
          <!-- Район? -->
          <div class="form-group">
            <label for="district">Район?</label>
            <select id="district" class="form-select" v-model="formData.district">
              <option disabled value="">Выберите район</option>
              <option v-for="item in districts" :key="item.id" :value="item.name">
                {{ item.name }}
              </option>
            </select>
          </div>
          <!-- Координаты -->
          <div class="form-group" style="height: 300px;">
            <label>Координаты</label>
            <l-map style="width: 100%;" :zoom="zoom" :center="center" @load="onMapLoad" @click="onMapClick">
              <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
              <l-marker v-if="markerLatLng" :lat-lng="markerLatLng" @update:lat-lng="updateCoordinates"></l-marker>
            </l-map>
          </div>

          <!-- Загрузить фотографии животного -->
          <div class="form-group" style="margin-top: 45px;">
            <h2 style="margin-top: 25px; font-weight: bold; margin-bottom: 10px;">Добавить фотографию питомца</h2>
            <input id="animalPhotos" type="file" class="form-control" @change="onFileChange" multiple />
          </div>

          <!-- Кнопка Создать -->
          <button class="btn btn-primary" @click="submitForm" style="margin-bottom: 25px;">Создать</button>
        </div>
      </div>
      <div class="col-md-6">
        <div>
          <img src="@/assets/cat2.png" class="w-100 img-fluid" alt="Your image description" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker } from 'vue3-leaflet';
import 'leaflet/dist/leaflet.css';

export default {
  name: 'AnnouncementCreate',
  components: {
    LMap,
    LTileLayer,
    LMarker,
  },
  data() {
    return {
      typeAnnouncement: [
        { id: 1, type: 'lost', name: 'Пропажа' },
        { id: 2, type: 'found', name: 'Находка' },
      ],
      animalType: [
        { id: 1, name: 'Кошка', type: 'cat' },
        { id: 2, name: 'Собака', type: 'dog' },
      ],
      animalBreeds: [],
      animalColors: [
        { id: 1, name: 'Белый' },
        { id: 2, name: 'Чёрный' },
        { id: 3, name: 'Коричневый' },
        { id: 4, name: 'Серый' },
      ],
      districts: [
        { id: 1, name: 'Алатауский район' },
        { id: 2, name: 'Алмалинский район' },
        { id: 3, name: 'Ауэзовский район' },
      ],
      formData: {
        animal: {
          animalName: '',
          type: '',
          breed: '',
          color: '',
          description: '',
        },
        coordinates: null,
        type: '',
        status: 'ACTIVE',
        phoneNumber: '',
        userId: '',
        details: '',
        district: '',
      },
      files: [],
      zoom: 13,
      center: { lat: 43.2220, lng: 76.8512 },
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      markerLatLng: { lat: 43.2220, lng: 76.8512 },
    };
  },
  methods: {
    onFileChange(event) {
      this.files = Array.from(event.target.files);
    },
    onAnimalTypeChange() {
      const selectedAnimal = this.animalType.find(item => item.name === this.formData.animal.type);
      if (selectedAnimal) {
        this.animalBreeds = [
          { id: 1, name: 'Сиамская', type: 'cat' },
          { id: 2, name: 'Персидская', type: 'cat' },
          { id: 3, name: 'Дворняжка', type: 'dog' },
        ].filter(breed => breed.type === selectedAnimal.type);
      } else {
        this.animalBreeds = [];
      }
    },
    submitForm() {
      if (!this.formData.type || !this.formData.animal.animalName || !this.formData.district) {
        alert('Пожалуйста, заполните все обязательные поля!');
        return;
      }

      // Локальная обработка данных
      console.log('Данные формы отправлены:', this.formData);
      console.log('Файлы:', this.files);
      alert('Объявление успешно создано!');

      // Очистка формы после успешной отправки
      this.formData = {
        animal: {
          animalName: '',
          type: '',
          breed: '',
          color: '',
          description: '',
        },
        coordinates: null,
        type: '',
        status: 'ACTIVE',
        phoneNumber: '',
        userId: '',
        details: '',
        district: '',
      };
      this.files = [];
    },
    onMapLoad() {
      this.$nextTick(() => {
        this.markerLatLng = this.center;
        this.formData.coordinates = this.markerLatLng;
      });
    },
    updateCoordinates(newVal) {
      this.formData.coordinates = newVal;
    },
    onMapClick(event) {
      this.markerLatLng = event.latlng;
      this.formData.coordinates = event.latlng;
    },
  },
};
</script>

<style scoped>
.create-lost-pet {
  text-align: left;
  font-weight: bold;
}
.create-lost-pet .form-group {
  margin-bottom: 1rem;
}
.create_details {
  justify-content: space-between;
  display: flex;
  padding: 0;
  margin: 0;
}
</style>
