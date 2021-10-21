<template>
  <div class="container-fluid">
    <input type="text" v-model="filtro">
    <div class="row">
      <Product :products="products" v-on:addItem="addItem" class="col-6" />
      <Cart :cart="cart" :total="total" class="col-6" />
    </div>
  </div>
</template>

<script>
import Product from "./components/Product";
import Cart from "./components/Cart";

export default {
  name: "App",
  data() {
    return {
      products: [],
      total: 0,
      filtro: ''
    };
  },

  components: { Product, Cart },
  computed: {
    productosFiltrados(){
        return  this.products.filter(product => product.includes(this.filtro));
      },
    cart() {
      let carro = this.products.filter((product) => product.quantity > 0);
      carro.sort(function (a, b) {
        let nameA = a.name.toUpperCase();
        let nameB = b.name.toUpperCase();
        if (nameA < nameB){
          return -1;
        } if (nameA > nameB){
          return 1;
        }
        return 0;
      });
      this.laCuenta(carro);
      return carro;
    },
    discount(product) {
      return (
        product.price -
        Number.parseFloat(product.price * (product.discount / 100)).toFixed(2)
      );
    },
  },
  methods: {
    getProducts() {
      fetch("http://localhost:3000/api/products")
        .then((res) => res.json())
        .then((data) => {
          data.forEach((c) => {
            c.quantity = 0;
            this.products.push(c);
          });
        });
    },
    addItem(product) {
      product.quantity++;
      console.log(product.quantity);
    },
    laCuenta(carro){      
      let fullPrice = 0;
      let onSale = 0;
      carro.forEach((p)=>{
        if(p.discount == 0){
          fullPrice = parseFloat(fullPrice) + (p.price*p.quantity);
        }else{
          onSale = parseFloat(onSale + ((p.price - (p.price * (p.discount/100)))*p.quantity));
        }
      })
      this.total = parseFloat(fullPrice + onSale).toFixed(2);
    }
  },
  mounted() {
    this.getProducts();
  },
};
</script>

<style>
</style>
