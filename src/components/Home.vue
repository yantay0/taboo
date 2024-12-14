<script>
import 'bootstrap/dist/js/bootstrap.min.js';
import { useRouter } from 'vue-router'; // Импортируем useRouter

export default {
  name: 'HeaderVue',
  setup() {
    const router = useRouter(); // Инициализируем роутер

    const navigateToAnnouncementCreate = () => {
      router.push('/announcementCreate'); // Переход на страницу "Подать объявление"
    };

    return { navigateToAnnouncementCreate };
  },
};
</script>

<template>
  <div class="container-fluid container-bg-home">
    <div class="container">
      <div class="row">
        <div class="col-md-6">
          <div class="text-heading">
            <h1>tabu.kz</h1>
            <h4>Верните Своего Друга Домой</h4>
            <p>
              Cайт предлагает вам помощь в размещении объявления о пропаже или находке питомца. Мы предоставляем инструменты и ресурсы, чтобы помочь вам распространить информацию в виде объявления и фотографии. Присоединяйтесь к нам сегодня, и мы поможем вам вернуть своего друга домой.
            </p>
            <button class="tablinks" @click.prevent="navigateToAnnouncementCreate" id="defaultOpen">
              Подать объявление
            </button>
          </div>
        </div>
        <div class="col-md-6">
          <div class="image-container-home">
            <img src="@/assets/sobaka.png" class="w-100 img-fluid" alt="Your image description" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
