<template>
  <div class="card">
    <img :src="item.img" alt="img" />
    <div class="title">{{ item.name }}</div>
    <div class="price">{{ parseInt(price) }}₽</div>
    <div v-if="item.discount" class="discount">{{ item.price }}₽</div>
    <div v-if="item.discount" class="discaunt-percent">
      {{ this.item.discount_percent }}%
    </div>
  </div>
</template>

<script>
export default {
  props: ["item"],
  computed: {
    price() {
      return this.item.discount
        ? this.item.price - (this.item.price * this.item.discount_percent) / 100
        : this.item.price;
    },
  },
};
</script>

<style lang="scss">
.card {
  display: flex;
  position: relative;
  flex-wrap: wrap;
  width: 18%;
  height: calc(100vh - 600px);
  padding: 10px;
  text-align: center;
  cursor: pointer;
  img,
  .title,
  .price,
  .discount {
    width: 100%;
  }
  img {
    max-height: 220px;
    transition: 0.5s;
    margin-bottom: 10px;
    &:hover {
      opacity: 0.5;
      transition: 0.5s;
    }
  }
  .price {
    font-weight: bold;
    font-size: 16px;
    line-height: 18px;
    color: black;
  }
  .discount {
    text-decoration: line-through;
    color: gray;
    font-size: 12px;
  }
  .discaunt-percent {
    position: absolute;
    top: 20px;
    left: 20px;
    width: 40px;
    height: 40px;
    background: #ecf8ff;
    border-radius: 50%;
    font-size: 12px;
    font-weight: bold;
    display: flex;
    justify-content: center;
    align-items: center;
  }
}
</style>
