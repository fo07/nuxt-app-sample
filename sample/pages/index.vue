<template>
  <section>
    <h2>検索結果 （{{shops.length}}件）</h2>
    <p v-if="error">データの取得に失敗しました</p>
    <ul>
      <li v-for="shop in shops" :key="shop.id">
        <p>{{shop.id}}</p>
        <p>{{shop.name}}</p>
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  layout: "test",
  data() {
    return {
      shops: [],
      error: false
    };
  },
  methods: {
    setShop(res) {
      this.shops = res.data.results.shop;
    },
    setError(err) {
      console.log(err);
      this.error = true;
    }
  },
  mounted() {
    navigator.geolocation.getCurrentPosition(position => {
      this.$axios("http://localhost:3000/api/gourmet/v1/", {
        params: {
          key: process.env.apikey,
          lat: position.coords.latitude,
          lng: position.coords.longitude,
          format: "json"
        }
      })
        .then(this.setShop)
        .catch(this.setError);
    }, this.setError);
  }
};
</script>
