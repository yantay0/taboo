<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-md-4" style="margin: 2% 0%;">
        <select v-model="selectedType" @change="filterMapDataByType" class="form-control" id="type">
          <option value="">Тип Объявления</option>
          <option v-for="typ in types" :key="typ"> {{ typ }} </option>
        </select>
      </div>
    </div>
    <div class="row">
      <div class="col-xl-12">
        <div style="height: 600px; width: 100%">
          <l-map v-if="filteredMapData.length > 0" :zoom="zoom" :center="center">
            <l-tile-layer :url="url"></l-tile-layer>
            <l-marker
              v-for="item in filteredMapData"
              :key="item.announcementId"
              :lat-lng="[Number(item.coordinates.lat), Number(item.coordinates.lng)]"
              @click="goToAnnouncement(item.announcementId)"
            ></l-marker>
          </l-map>
          <div v-else>Loading map...</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import { LMap, LTileLayer, LMarker } from 'vue3-leaflet';
// import { useRouter } from 'vue-router';
import 'leaflet/dist/leaflet.css';

export default {
  components: {
    LMap,
    LTileLayer,
    LMarker,
  },
  setup() {
    const url = 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png';
    const zoom = ref(13);
    const center = ref([43.23798106012807, 76.88850402832033]);

    // Локальные данные объявлений
    const mapData = ref([
      {
        announcementId: 1,
        type: 'FOUND',
        coordinates: { lat: 43.238, lng: 76.889 },
      },
      {
        announcementId: 2,
        type: 'MISSING',
        coordinates: { lat: 43.239, lng: 76.890 },
      },
      {
        announcementId: 3,
        type: 'GIVE',
        coordinates: { lat: 43.240, lng: 76.891 },
      },
      {
        announcementId: 4,
        type: 'FROM_SHELTER',
        coordinates: { lat: 43.241, lng: 76.892 },
      },
    ]);

    // Типы объявлений и их отображение на русском языке
    const typeNames = {
      FOUND: 'Найденные животные',
      MISSING: 'Пропавшие животные',
      GIVE: 'В добрые руки',
      FROM_SHELTER: 'Животные в приютах',
    };

    // Конвертируем типы в человекочитаемый формат
    mapData.value.forEach((item) => {
      item.type = typeNames[item.type];
    });

    // Уникальные типы объявлений
    const types = ref([...new Set(mapData.value.map((item) => item.type))]);

    const selectedType = ref('');
    // const router = useRouter();

    // Отфильтрованные данные карты
    const filteredMapData = computed(() => {
      if (!selectedType.value) {
        return mapData.value;
      }
      return mapData.value.filter((item) => item.type === selectedType.value);
    });

    // Переход к конкретному объявлению
    const goToAnnouncement = (announcementId) => {
      alert(`Перейти к объявлению ID: ${announcementId}`);
      // router.push(`/pet/${announcementId}`);
    };

    // Фильтрация по типу (обновляется автоматически через computed)
    const filterMapDataByType = () => {
      // Реактивные данные обновят карту
    };

    return { url, zoom, center, filteredMapData, goToAnnouncement, selectedType, types, filterMapDataByType };
  },
};
</script>
