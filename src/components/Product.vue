<template>
  <div class="container">
    <h1>TIENDA LAS GALLETAS</h1>
    <div class="row">
        
    </div>
    <div class="row">
      <p class="font-weight-bold text-uppercase">Productos</p>
    </div>
    <div class="row">
      <div
        class="col-xs-12 col-md-3 product bg-light border border-secondary m-1"
        v-for="product in products"
        :key="product.id"
      >
        <p>{{ product.name }}</p>
        <p v-if="product.discount == 0" class="d-none">
          {{ product.discount }}
        </p>
        <p v-else-if="product.discount < 20">{{ product.discount }}</p>
        <p v v-else class="alert-info">{{ product.discount }}</p>
        <p class="text-right" v-if="product.discount == 0">
          {{ product.price }}
        </p>
        <p class="text-right" v-else>{{ discount(product) }}</p>
        <button class="bg-primary text-white" v-on:click="addItem(product)">
          Comprar
        </button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "Product",  
  props: ["products"],
  computed:{
      
  },
  methods: {
    addItem(product) {
      this.$emit("addItem", product);
    },
    discount(product) {
      return Number.parseFloat(
        product.price -
          Number.parseFloat(product.price * (product.discount / 100)).toFixed(2)
      ).toFixed(2);
    },
  },
  mounted() {},
};
</script>
<style>
input{
    margin-left: 5px;
}
</style>