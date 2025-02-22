<template>
  <div>
    <section class="item-contain">
      <section class="img">
        <img :src="`/products/${product.img}`" />
      </section>
      <section class="product-info">
        <h1>{{ product.name }}</h1>
        <star-rating
          :rating="product.starrating"
          :star-size="15"
          :show-rating="false"
          active-color="#000"
          style="margin: 5px 0"
        ></star-rating> <span class = "greetClass" @click = "greet(product.description)">🔉</span>
        <h4 class="price">{{ 'Rs. ' + product.price  }}</h4>
        <p>{{ product.description }}</p>
        <div class="product-options">
          <div class="quantity">
            <button class="update-num" @click="quantity > 0 ? quantity-- : quantity = 0">-</button>
            <input type="number" v-model="quantity" />
            <button class="update-num" @click="quantity++">+</button>
          </div>
          <div v-if="product.sizes" class="size">
            <select v-model="size" class="size-picker" @change="showSizeRequiredMessage = false">
              <option :value="null" disabled hidden>Size</option>
              <option v-for="(size, key) in product.sizes" :key="key" :value="size">{{ size }}</option>
            </select>
          </div>
        </div>
        <p v-if="showSizeRequiredMessage" class="size-required-message">Please choose a size</p>
        <p>
          Available in additional colors:
          <strong>
            <span :style="`color: ${product.color}`">{{ product.color }}</span>
          </strong>
        </p>
        <p>
          <button class="button purchase" @click="cartAdd">Add to Cart</button>
        </p>
      </section>
    </section>
    <hr />
    <div class="review">
      <h2>Reviews</h2>
      <!-- maybe an image of a person? -->
      <star-rating
        :rating="product.starrating"
        active-color="#000"
        :star-size="15"
        :show-rating="false"
        style="margin: 5px 0"
      ></star-rating>
      <p>{{ product.review }}</p>
    </div>
    <app-featured-products />
  </div>
</template>

<script>
import { mapState } from "vuex";
import StarRating from "vue-star-rating/src/star-rating.vue";
import AppFeaturedProducts from "~/components/AppFeaturedProducts.vue";

export default {
  components: {
    StarRating,
    AppFeaturedProducts
  },
  data() {
    return {
      id: this.$route.params.id,
      quantity: 1,
      size: null,
      showSizeRequiredMessage: false,
      tempcart: [] // this object should be the same as the json store object, with additional params, quantity and size
    };
  },
  computed: {
    ...mapState(["storedata"]),
    product() {
      return this.storedata.find(el => el.id === this.id);
    }
  },
  methods: {
    greet(descr){
      let msg =  new SpeechSynthesisUtterance(descr);
      window.speechSynthesis.speak(msg);
    },
    cartAdd() {
      if (this.product.sizes && !this.size) {
        this.showSizeRequiredMessage = true;
        return;
      }

      let item = this.product;
      item = { 
        ...item, 
        quantity: this.quantity, 
        size: this.size 
      };
      this.tempcart.push(item);
      this.$store.commit("addToCart", {...item});
    }
  }
};
</script>

<style lang="scss" scoped>

.greetClass{
  cursor: pointer;
}

.item-contain {
  margin-left: 8%;
  width: 80%;
  display: grid;
  justify-content: space-around;
  grid-template-columns: 1fr 2fr;
}

.product-options {
  display: flex;
}

input,
select {
  width: 60px;
  font-size: 25px;
  margin: 0 5px;
  padding: 5px 10px;
}

.update-num {
  background: black;
  border-color: black;
  color: white;
  font-size: 20px;
  width: 45px;
}

.size {
  margin-left: 10px;
}

.size-picker {
  width: 130px;
  font-size: 20px;
  height: 100%;
  border: 0;
  background-color: white;
  outline: 1px solid #ccc;
  outline-offset: -1px;
}

.quantity {
  display: flex;
}

.size-required-message {
  color: red;
}

@media screen and (max-width: 650px) {
  .img img {
    width: 100%;
  }

  .item-contain {
    margin-left: 0 !important;
    width: 95% !important;
  }

  .review {
    width: 90%;
    margin-left: 4%;
  }
}
</style>
