<template>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <img class="mars" src="/static/mars.png" alt="Марс">
  <img class="ship" src="/static/ship.png" alt="Ракета">
  <div class="menu">
    <div class="burger-menu" @click="changeMenu()">
      <div class="bar"></div>
      <div class="bar"></div>
      <div class="bar"></div>
    </div>
    <ul class="point-menu-burger" ref="burger" @click="changeMenu()">
      <li class="menu-item" v-for="(title,index) in menu" :key="index"><a href="#">{{ title.text }}</a></li>
    </ul>
    <div class="logo">
      <img class="spacex" src="/static/лого.png" alt="Логотип">
    </div>
    <ul class="point-menu">
      <li class="menu-item" v-for="(title,index) in menu" :key="index"><a href="#">{{ title.text }}</a></li>
    </ul>
  </div>
  <div class="app" ref="app">
    <router-view />
  </div>
</template>

<script>
import {All_MENU, SITE_INFO} from "@/queries";

export default {
  name: "AppView",
  data() {
    return {
      mySite: null,
      menu: [
        //{text:'Главная'}, {text:'Технология'}, {text:'График полетов'}, {text:'Гарантии'}, {text:'О компании'}, {text:'Контакты'}
      ],
      check_menu: false,
    };
  },
  methods: {
    changeMenu() {
      const burger = this.$refs.burger;
      const app = this.$refs.app;
      if (!this.check_menu) {
        app.classList.add('fade-out');
        app.classList.add('hidden');

        setTimeout(() => {
          app.style.display = 'none';

          burger.style.display = 'flex';
          burger.classList.add('fade-in');
          burger.classList.add('visible');
        }, 500);

        this.check_menu = !this.check_menu;
      } else {
        const classes = ['.title','.table','.line','.start-btn']
        classes.forEach(item => {this.$refs.app.querySelector(item).style.animationDelay = '0s';})
        
        burger.classList.remove('fade-in');
        burger.classList.remove('visible');
        burger.classList.add('fade-out');
        burger.classList.add('hidden');

        setTimeout(() => {
          burger.style.display = 'none';

          app.style.display = 'block';
          app.classList.add('fade-in');
          app.classList.add('visible');
        }, 250);

        this.check_menu = !this.check_menu;
      }

    },
  },
  async created() {
    const menu = await this.$apollo.query({
      query: All_MENU,
    });
    this.menu = menu.data.all_menu;
  },
}
</script>

