<script>
import { mask } from 'vue-the-mask';

export default {
  directives: { mask },
  name: 'AuthenticateForm',
  data() {
    return {
      // Локальное хранилище пользователей
      users: [
        {
          phone: '+7 (777) 777-7777',
          password: 'defaultpassword',
          firstName: 'Default User',
          shelter: false,
        },
      ],
      user: {
        phone: '',
        password: '',
        firstName: '',
        shelter: false,
      },
      confirmPassword: '',
    };
  },
  methods: {
    authenticate() {
      // Проверяем пользователя в локальных данных
      const user = this.users.find(
        (u) => u.phone === this.user.phone && u.password === this.user.password
      );
      if (user) {
        alert(`Добро пожаловать, ${user.firstName || 'Пользователь'}!`);
        localStorage.setItem('jwtToken', 'fake-token'); // Имитация токена
        this.$router.push('/'); // Переход на главную страницу
      } else {
        alert('Неверный номер телефона или пароль');
      }
    },
    register() {
      // Проверяем совпадение паролей
      if (this.user.password !== this.confirmPassword) {
        alert('Пароли не совпадают');
        return;
      }

      // Проверяем, существует ли пользователь
      const exists = this.users.some((u) => u.phone === this.user.phone);
      if (exists) {
        alert('Пользователь с таким номером уже зарегистрирован');
        return;
      }

      // Добавляем нового пользователя
      this.users.push({ ...this.user });
      alert('Регистрация успешна!');
      this.user = { phone: '', password: '', firstName: '', shelter: false }; // Сбрасываем форму
      this.confirmPassword = '';
      this.openTab('auth'); // Переходим на вкладку авторизации
    },
    openTab(tabName) {
      const tabcontent = document.getElementsByClassName('tabcontent');
      for (let i = 0; i < tabcontent.length; i++) {
        tabcontent[i].style.display = 'none';
      }

      const tablinks = document.querySelectorAll('.tablinks');
      tablinks.forEach((tab) => tab.classList.remove('active'));

      if (tabName === 'auth') {
        tablinks[0].classList.add('active');
      } else {
        tablinks[1].classList.add('active');
      }

      document.getElementById(tabName).style.display = 'block';
    },
  },
  mounted() {
    this.$nextTick(() => {
      document.getElementById('defaultOpen').click();
    });
  },
};
</script>

<template>
  <div class="container-fluid container-bg">
    <div class="row d-flex justify-content-center align-items-center">
      <div class="col-lg-4 col-sm-0">
        <div class="image-container">
          <img src="@/assets/fox.png" class="w-100 img-fluid" alt="Your image description" />
        </div>
      </div>
      <div class="col-md-8 col-sm-12">
        <div class="login-container">
          <h2 class="auth-text">Авторизация</h2>
          <div class="tab">
            <button class="tablinks" @click.prevent="openTab('auth')" id="defaultOpen">Вход</button>
            <button class="tablinks" @click.prevent="openTab('register')">Регистрация</button>
          </div>
          <div id="auth" class="tabcontent">
            <form @submit.prevent="authenticate">
              <div class="form-group">
                <input
                  type="tel"
                  v-model="user.phone"
                  id="phone"
                  v-mask="'+7 (###) ###-####'"
                  placeholder="+7 (___) ___-____"
                />
              </div>
              <div class="form-group">
                <input type="password" v-model="user.password" id="password" placeholder="Введите пароль" />
              </div>
              <button type="submit">Войти</button>
            </form>
            <div class="forgot-password">
              <a href="#">Forgot your password?</a>
            </div>
          </div>
          <div id="register" class="tabcontent">
            <form @submit.prevent="register">
              <div class="form-group-checkbox">
                <input type="checkbox" v-model="user.shelter" />
                <label for="shelter" id="shelter">Приют:</label>
              </div>
              <div class="form-group">
                <label for="firstName"></label>
                <input
                  type="text"
                  id="firstName"
                  v-model="user.firstName"
                  required
                  placeholder="Имя/Название:"
                />
              </div>
              <div class="form-group">
                <input
                  type="tel"
                  v-model="user.phone"
                  id="phone"
                  v-mask="'+7 (###) ###-####'"
                  placeholder="Номер телефона"
                />
              </div>
              <div class="form-group">
                <input
                  type="password"
                  id="password"
                  v-model="user.password"
                  required
                  placeholder="Пароль"
                />
              </div>
              <div class="form-group">
                <input
                  type="password"
                  id="confirmPassword"
                  v-model="confirmPassword"
                  required
                  placeholder="Подтвердите пароль:"
                />
              </div>
              <button type="submit">Регистрация</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
