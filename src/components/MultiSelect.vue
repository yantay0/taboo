<template>
  <div class="text-decor">
    <div class="form-group">
      <label for="typeAnnouncement">Вид объявления</label>
      <select
        id="typeAnnouncement"
        class="form-select"
        v-model="selectedTypeAnnouncement"
      >
        <option disabled value="">Выберите тип объявления</option>
        <option
          v-for="item in typeAnnouncement"
          :key="item.id"
          :value="item.type"
        >
          {{ item.name }}
        </option>
      </select>
    </div>

    <div class="form-group">
      <label for="animalType">Вид животного</label>
      <select id="animalType" class="form-select" v-model="selectedAnimalType">
        <option disabled value="">Выберите вид животного</option>
        <option v-for="item in animalType" :key="item.id" :value="item.type">
          {{ item.name }}
        </option>
      </select>
    </div>

    <div class="form-group">
      <label for="animalBreed">Порода животного</label>
      <select
        id="animalBreed"
        class="form-select"
        v-model="selectedAnimalBreed"
        :disabled="!animalBreed.length"
      >
        <option disabled value="">Выберите породу животного</option>
        <option v-for="item in animalBreed" :key="item.id" :value="item.name">
          {{ item.name }}
        </option>
      </select>
    </div>

    <p>
      Если у вас пропал питомец или же вы нашли его вы можете подать объявление о пропаже или находке собаки на нашем сайте. Постарайтесь дать как можно более полную информацию об обстоятельствах пропажи/находки. Прикрепите одну или несколько фото к объявлению, это существенно повысит шансы на успех.
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedTypeAnnouncement: null,
      selectedAnimalType: null,
      selectedAnimalBreed: null,
      typeAnnouncement: [
        { id: 1, type: 'found', name: 'Найден' },
        { id: 2, type: 'lost', name: 'Потерян' },
        { id: 3, type: 'adopt', name: 'Отдать в добрые руки' },
      ],
      animalType: [
        { id: 1, type: 'dog', name: 'Собака' },
        { id: 2, type: 'cat', name: 'Кошка' },
        { id: 3, type: 'bird', name: 'Птица' },
      ],
      animalBreed: [],
      allBreeds: {
        dog: [
          { id: 1, name: 'Лабрадор' },
          { id: 2, name: 'Бульдог' },
          { id: 3, name: 'Немецкая овчарка' },
        ],
        cat: [
          { id: 4, name: 'Сиамская' },
          { id: 5, name: 'Мейн-кун' },
          { id: 6, name: 'Бенгальская' },
        ],
        bird: [
          { id: 7, name: 'Попугай' },
          { id: 8, name: 'Канарейка' },
          { id: 9, name: 'Волнистый попугайчик' },
        ],
      },
    };
  },
  watch: {
    selectedAnimalType(newVal) {
      if (newVal) {
        this.animalBreed = this.allBreeds[newVal] || [];
      } else {
        this.animalBreed = []; // Очистить породы животных при сбросе выбора
      }
    },
  },
};
</script>

<style scoped>
.multiselect {
  color: #000;
  display: flex;
  flex-direction: column;
}

.multiselect-filter {
  text-align: left;
  color: #000;
  max-width: 500px;
}

.text-decor {
  color: #fff;
  text-align: initial;
  font-size: 20px;
  font-family: 'Raleway', sans-serif;
}

select {
  margin-bottom: 10px;
}
</style>
