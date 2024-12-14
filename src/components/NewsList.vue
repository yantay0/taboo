<template>
  <div class="container" style="text-align: left; margin-top: 30px;">
    <div class="row">
      <div class="col-md-4" style="margin-bottom: 3%;">
        <label for="category" class="text-panel">Категория:</label>
        <select v-model="selectedCategory" @change="filterNewsByCategory" class="form-select" id="category">
          <option value="">Все</option>
          <option v-for="category in categories" :key="category">{{ category }}</option>
        </select>
      </div>
      <div class="col-md-4">
        <label for="search" class="text-panel">Поиск по заголовку:</label>
        <input v-model="searchText" @input="filterNewsByTitle" type="text" class="form-control" id="search"
          placeholder="Введите заголовок...">
      </div>
    </div>
    <div class="row row-cols-1 row-cols-md-2 g-4">
      <div class="col-md-8 col-sm-12 col-xl-8" v-for="news in newsData" :key="news.id">
        <div class="card">
          <img class="card-img-top " :src="news.cover_photo" alt="Cover photo">
          <div class="card-body">
            <h4 class="card-title"> <b>{{ news.title }}</b></h4>
            <p class="card-text">{{ news.description }}</p>
            <p class="card-date">{{ news.created_date }}</p>
          </div>
          <div class="card-footer justify_details ">
            <p class="card-author">Автор статьи: {{ news.author }}</p>
            <p class="card-category">{{ news.category }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newsData: [
        {
          id: 1,
          title: "Помогите найти Барсика",
          description: "Барсик пропал в районе Центрального парка. Отзывается на имя и очень дружелюбен.",
          created_date: "2024-12-12",
          author: "Анна Иванова",
          category: "Пропажа",
          cover_photo: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRRvsDf-IQAA7fwqsHRfAgzcGS2JawxvB9ffg&s"
        },
        {
          id: 2,
          title: "Найден щенок в Алмалинском районе",
          description: "Найден щенок без ошейника, белый с черными пятнами. Звоните, если это ваш питомец.",
          created_date: "2024-12-10",
          author: "Петр Сидоров",
          category: "Находка",
          cover_photo: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRZb8i9L55rCdUbZgraldDDzyaqSFJ7oi5HPw&s"
        },
        {
          id: 3,
          title: "Как правильно искать пропавшего питомца",
          description: "Советы и рекомендации по поиску пропавшего друга.",
          created_date: "2024-12-05",
          author: "Екатерина Смирнова",
          category: "Советы",
          cover_photo: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRmmRckjZqCx6JLwBu4agGYUNaDzEZiDeIQNA&s"
        }
      ],
      allNews: [],
      selectedCategory: '',
      searchText: '',
      categories: []
    };
  },
  created() {
    this.allNews = [...this.newsData];
    this.categories = [...new Set(this.newsData.map(news => news.category))];
  },
  methods: {
    filterNewsByCategory() {
      if (this.selectedCategory) {
        this.newsData = this.allNews.filter(news => news.category === this.selectedCategory);
      } else {
        this.newsData = [...this.allNews];
      }
    },
    filterNewsByTitle() {
      this.newsData = this.allNews.filter(news => news.title.toLowerCase().includes(this.searchText.toLowerCase()));
    }
  }
};
</script>

<style scoped>
.text-panel {
  color: #000000;
  text-align: initial;
  font-size: 20px;
  font-family: 'Raleway', sans-serif;
}
</style>
