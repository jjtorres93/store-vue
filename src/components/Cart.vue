<template>
   <div class="col-6">
        <p class="font-weight-bold text-uppercase">Carrito</p>
                    <table class="table">
                        <thead>
                            <tr>
                                <th scope="col">id</th>
                                <th scope="col">Nombre</th>
                                <th scope="col">Precio</th>
                                <th scope="col">Cantidad</th>
                                <th scope="col">Subtotal</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="product in cart" :key="product.id">
                                <td>{{product.id}}</td>
                                <td>{{product.name}}</td>
                                <td>{{discount(product)}} €</td>
                                <td>{{product.quantity}}</td>
                                <td>{{subtotal(product)}} €<button
                                 v-on:click="quitar(product)">Eliminar</button></td>
                            </tr>
                        </tbody>
                        <tfoot>
                            <tr>
                                <td>Total: {{total}} €</td>
                            </tr>
                        </tfoot>
                        </table>
       </div> 
</template>
<script>
export default {
     name:"Cart",
     props:['cart','total'],
     methods:{
         subtotal(product){
            return Number.parseFloat(this.discount(product) * product.quantity).toFixed(2);
         },
         quitar(product){             
             product.quantity=0;
         },
         discount(product){
             return product.price - Number.parseFloat(product.price * (product.discount / 100)).toFixed(2);
         }
     }
};
</script>