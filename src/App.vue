<template>
  <div id="app" class="card-container">
    <Product v-for="(item, idx) in items" :key="idx" :item="item" />
    <div v-if="this.items.length !== 0" class="btn-containter">
      <button @click="fetchMore">
        {{ isAllRendered ? "БОЛЬШЕ НЕЧЕГО ПОДГРУЗИТЬ" : "ПОКАЗАТЬ ЕЩЁ" }}
      </button>
    </div>
    <div v-else class="no-data">Товаров нет</div>
  </div>
</template>

<script>
import Product from "./components/Product.vue";

export default {
  name: "App",
  components: {
    Product,
  },
  computed: {
    isAllRendered() {
      if (this.items.length !== 0) {
        return this.items[this.items.length - 1].id === 89 ? true : false;
      } else {
        return false;
      }
    },
  },
  data() {
    return {
      items: [],
      perPage: 10
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      return fetch(
        `https://client.cvetochki.tk/items?expand=itemPictures&filter[latitude]=55.7540471&filter[longitude]=37.620405&per-page=${this.perPage}`
      )
        .then((response) => response.json())
        .then((commits) => {
          console.log(commits.data);
          this.items = [];
          commits.data.forEach((el) => {
            let temp = {
              id: el.id,
              name: el.name,
              price: el.price,
              discount: el.discount,
              discount_percent: el.discount_percent,
              img: el.itemPictures[0].picture_url,
            };
            this.items.push(temp);
          });
          console.log(this.items);
        });
    },
    fetchMore() {
      this.perPage += 10;
      this.fetchData();
    },
  },
};
</script>

<style lang="scss">
* {
  font-family: Arial;
  font-style: normal;
}
.card-container {
  display: flex;
  justify-content: center;
  padding: 100px;
  flex-wrap: wrap;
  .btn-containter {
    width: 100%;
    display: flex;
    justify-content: center;
    button {
      background: #59c3ff;
      border-radius: 4px;
      padding: 10px;
      border: none;
      font-weight: bold;
      cursor: pointer;
      transition: 0.5s;
      &:hover {
        color: #59c3ff;
        border: 1px solid #59c3ff;
        background: white;
        transition: 0.5s;
      }
    }
  }
}
</style>
