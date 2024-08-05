<template>

  <div class="block">
    <div class="line">
      <button class="start-btn"><span class="span-left"/>Начать путешествие<span class="span-right"/></button>
      <svg class="svg-line" :width="width_box" height="130" :viewBox="view_box">
        <path :d="path" stroke="url(#grad)" :stroke-width="width_line" fill="transparent"/>
        <defs>
          <linearGradient id="grad" x1="0%" y1="0%" x2="100%" y2="0%">
            <stop offset="0%" stop-color="white" stop-opacity="0" />
            <stop offset="100%" stop-color="white" stop-opacity="1" />
          </linearGradient>
        </defs>
      </svg>
    </div>
    <div class="title">
      <h1 class="adventure">
        ПУТЕШЕСТВИЕ<p class="small">на красную планету</p>
      </h1>
    </div>
    <ul class="table">
      <li class="grid" v-for="(text,index) in benefits" :key="index">
        <p class="text">{{text.top_text}}</p>
        <p class="integer" v-html="text.integer"></p>
        <p class="text">{{text.bot_text}}</p>
      </li>
    </ul>
  </div>
</template>

<script>
import {All_BENEFITS, All_MENU} from "@/queries";

export default {
  name: "home",
  data() {
    return {
      benefits: [
        // {
        //   top: 'мы',
        //   center: '1',
        //   bottom: 'на рынке',
        // },
        // {
        //   top: 'гарантируем',
        //   center: '50%',
        //   bottom: 'безопасность',
        // },
        // {
        //   top: 'календарик за',
        //   center: `2001<span style="font-size: 1.1vw;">г.</span>`,
        //   bottom: 'в подарок',
        // },
        // {
        //   top: 'путешествие',
        //   center: '597',
        //   bottom: 'дней',
        // },
      ],
      path: '',
      width_line: '',
      view_box: '',
      width_box: '',
      heigth_box:'',
    };
  },
  mounted() {
    this.updatePath();
    window.addEventListener('resize', this.updatePath);
  },
  methods: {
    updatePath() {
      const svgWidth = document.body.clientWidth;
      if (svgWidth > 960) {
        const endLine = (svgWidth * 0.34).toFixed(0)
        const radius = (svgWidth * 0.0026).toFixed(2)

        this.path = `M0,50 L140,140 L${endLine},140 A${radius},${radius} 0 1,1 ${parseFloat(endLine) + 0.01},140.1`;
        this.width_line = `${(svgWidth * 0.0013).toFixed(0)}`
        this.view_box = `0 0 ${parseFloat(endLine)+parseFloat(radius)*2} 150`
        this.width_box = `${(svgWidth * 0.297).toFixed(0)}`
      } else {
        const endLine = (svgWidth * 0.74).toFixed(0)
        const radius = (svgWidth * 0.0086).toFixed(2)

        this.path = `M0,50 L240,240 L${endLine},240 A${radius},${radius} 0 1,1 ${parseFloat(endLine) + 0.01},240.1`;
        this.width_line = `${(svgWidth * 0.0063).toFixed(0)}`
        this.view_box = `0 0 ${parseFloat(endLine)+parseFloat(radius)*2} 250`
        this.width_box = `${(svgWidth * 0.421).toFixed(0)}`
      }

      // console.log(this.path, '\n width_line', this.width_line, '\n view_box', this.view_box, '\n width_box', this.width_box, '\n svgWidth', svgWidth)
    },
  },
  beforeUnmount() {
    window.removeEventListener('resize', this.updatePath);
  },
  async created() {
    try {
      const benefits = await this.$apollo.query({
        query: All_BENEFITS,
      });
      this.benefits = benefits.data.all_benefits;
    } catch (error) {
      console.error('Ошибка при создании хука:', error);
    }
  }
};
</script>

<style>

</style>