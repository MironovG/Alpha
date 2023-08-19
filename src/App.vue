<template>
  <div>
    <header class="header">
      <div class="wrapper">
        <img src="./assets/logo.png" alt="logo" class="logo">
      </div>
    </header>
    <div class="header_top">
      <div class="wrapper">
        <h1 class="header__text">Новости</h1>
      </div>
    </div>
    <div class="container">
      <div v-for="piece in visibleNews" :key="piece.id" class="news">
        <img v-if="piece.image" :src="piece.image" alt="picture" class="news__image">
        <div class="news__date">
          <div class="day">{{ getDay(piece.date) }}</div>
          <div class="month__year">{{ getMonth(piece.date) }} <span>{{ getYear(piece.date) }}</span></div>
        </div>
        <div class="news__text">{{ piece.name }}</div>
        <div class="preview">{{ piece.previewText }}</div>
        <a href="#" class="news__link">Новости</a>
      </div>
      <div class="news__btn-container">
        <button class="news__btn" @click="loadMoreNews" v-if="hasMoreNews">Загрузить еще</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      news: [],
      visibleNews: [],
      hasMoreNews: true,
      loadedNewsCount: 0
    }
  },
  async mounted() {
    await this.loadNews();
  },
  methods: {
    async loadNews() {
      try {
        const response = await fetch('http://flems.github.io/test/api/news?offset=' + this.loadedNewsCount);
        const data = await response.json();
        if (data.items.length > 0) {
          this.news = data.items;
          this.loadedNewsCount = this.news.length;
          this.visibleNews = this.news.slice(0, 5);
          if (this.news.length <= 5) {
            this.hasMoreNews = false;
          }
        } else {
          this.hasMoreNews = false;
        }
      } catch (error) {
        console.error('Произошла ошибка при загрузке новостей!');
      }
    },
    loadMoreNews() {
      const startIndex = this.visibleNews.length;
      const endIndex = Math.min(this.news.length, startIndex + 5);
      this.visibleNews = this.visibleNews.concat(this.news.slice(startIndex, endIndex));
      if (endIndex === this.news.length) {
        this.hasMoreNews = false;
      }
    },
    getDay(timestamp) {
      return new Date(timestamp * 1000).toLocaleDateString('en-US', { day: 'numeric' });
    },
    getMonth(timestamp) {
      return new Date(timestamp * 1000).toLocaleDateString('en-US', { month: 'long' });
    },
    getYear(timestamp) {
      return new Date(timestamp * 1000).toLocaleDateString('en-US', { year: 'numeric' });
    }
  }
}
</script>

<style>

*{
margin: 0;
padding: 0;
box-sizing: border-box;
font-family: 'Nunito Sans', sans-serif;
}

.wrapper{
  max-width: 1710px;
  margin: 0 auto;
}

.logo{
  width: 152px;
  height: 48px;
}
.header_top{
  background-image: url(assets/alpha.png);
  background-position: center;
  min-height: 320px;
  display: flex;
  align-items: flex-end;
  padding-bottom: 48px;
}
.header_top .wrapper{
  width: 100%;
}
.container{
  max-width: 1710px;
  display: flex;
  flex-wrap: wrap;
  padding-top: 64px;
  justify-content: center;
}
.news{
 max-width: 500px;
  min-height: 554px;
  border: 1px solid #0F62FE;
  display: flex;
  flex-direction: column;
  border-radius: 16px;
  align-items: flex-start;
  margin-right: 48px;
  margin-bottom: 64px;
  justify-content: space-between;
}
.news__image{
height: 250px;
border-radius: 16px;
width: 100%;
}
.news__date{
  display: flex;
  align-self: flex-start;
}
.day{
  color: #A1A7B5;
  font-size: 36px;
  line-height: 36px;
  font-weight: 400;
  margin-right: 4px;
}
.month__year{
  font-size: 15px;
  line-height: 16.5px;
  font-weight: 700;
  color: #A1A7B5;
  display: flex;
  flex-direction: column;
}
.news__date{
  padding: 18px 32px;
}
.news__text{
  color: #0029A9;
  padding: 0px 32px;
  font-size: 22px;
  line-height: 26.4px;
  font-weight: 400;
}
.preview{
  font-size: 20px;
  line-height: 26px;
  font-weight: 400;
  color: #222327;
  padding: 18px 32px;
}
.news__link{
  color: #00133A;
  background-color: #F0F6FE;
  padding: 4px 16px;
  border-radius: 20px;
  text-decoration: none;
  margin-left: 18px;
  margin-bottom: 32px;
}
.news__btn-container {
  width: 100%;
  text-align: center;
  margin-top: 32px;
}
.news__btn{
  color: #002DBF;
  border-radius: 8px;
  padding: 16px 32px;
  border-color: #002DBF;
  font-weight: 600;
  font-size: 20px;
  background: transparent;
  line-height: 24px;
  margin-bottom: 20px;
}
</style>

