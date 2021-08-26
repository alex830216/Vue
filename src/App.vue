<template>
  <main>
    

    <section class="container mt-4">
      <div class="row items">
        <div class="col-sm-2" v-for="c in cats" :key="c">
          <div class="card" data-product-id="624946E">
            <img :src="`/images/${c.photo}`" class="card-img-top" alt="c.name" />
            <div class="card-body">
              <h5 class="title card-title fw-light fs-6">{{c.name}} </h5>
              <p class="price">${{c.price}} </p>
              <button class="btn btn-sm btn-warning fw-light" @click="addToCart(c)">
                <i class="fas fa-cat"></i>
              </button>
            </div>
          </div>
        </div>
      </div>
      <hr />
      

      <section class="cart">
        <h2>購物車</h2>
        <table class="table cart-item-table">
          <thead>
            <tr>
              <th scope="col">項目</th>
              <th scope="col">數量</th>
              <th scope="col">單價</th>
              <th scope="col">小計</th>
              <th scope="col"></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="cart in shoppingCart" :key="cart">
              <td>{{ cart.name }} </td>
              <td><input type="number" class="quantity" v-model="cart.qty" /></td>
              <td>${{ cart.price }}</td>
              <td>${{ cart.price * cart.qty }}</td>
              <td>
                <button class="remove-item-btn btn btn-danger btn-sm">
                  <i class="fas fa-trash-alt"></i>
                </button>
              </td>
            </tr>
          </tbody>
          <tfoot>
            <tr>
              <td colspan="2"></td>
              <td>總價</td>
              <td><span class="total-price">${{totalPrice}}</span></td>
              <td></td>
            </tr>
          </tfoot>
        </table>
        <button class="btn btn-lg btn-success empty-cart">
          <i class="fas fa-baby-carriage"></i> 清空購物車
        </button>
      </section>
    </section>
  </main>
</template>

<script>
  import { computed, ref } from "vue";
  import headerBlock from "./"
  export default {
    setup() {
      const cats = ref([])
      const shoppingCart = ref([])

      const addToCart = (item) => {
        const targetItem = shoppingCart.value.filter((d) => d.id === item.id)
        if(targetItem.length > 0 ) {
          targetItem[0].qty++
        }
        else {
          shoppingCart.value.push({
            ...item,
            qty: 1,
          })
        }
      }

      const totalPrice = computed(() => {
        return shoppingCart.value.reduce((sum,item) => {
          return Math.floor(sum + item.qty * item.price * 100) / 100
        },0)
      })

      }
      fetch("/cat.json")
        .then((res) => res.json())
        .then((data) => {
          cats.value = data
        })
      
      return {
        cats,
        shoppingCart,
        addToCart,
        totalPrice,
      }
    }
  };
</script>