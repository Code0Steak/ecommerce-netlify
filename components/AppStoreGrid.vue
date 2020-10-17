<template>
  <div class="storegrid">
    <transition-group name="items" tag="section" class="content">
      <div v-for="item in filteredprice" :key="item.id" class="item">
          <div class = "card">
          <div class="img-contain">
            <NuxtLink :to="`product/${item.id}`">
              <img :src="`/products/${item.img}`" />
            </NuxtLink>
          </div>
          <star-rating
            :rating="item.starrating"
            active-color="#000"
            :star-size="15"
            :show-rating="false"
            style="margin: 5px 0"
          ></star-rating>
          <h3>{{ item.name }}</h3>
          <h4 class="price">{{ 'Rs. ' + item.price  }}</h4>
          <NuxtLink :to="`product/${item.id}`">
            <button class="multi-item">View Item ></button>
          </NuxtLink></div>
      </div>
    </transition-group>
    <aside>
       <h3>Filter by Price:</h3>
      <p style="margin-top: 5px">
        Max Price
        <strong>Rs. {{ pricerange }}</strong>
      </p>
      <input
        class="slider"
        id="pricerange"
        type="range"
        v-model="pricerange"
        :min="min"
        :max="max"
        step="0.1"
      />
      <span class="min">Rs. {{ min }}</span>
      <span class="max">Rs. {{ max }}</span>
    </aside>
  </div>
</template>

<script>
import StarRating from "vue-star-rating/src/star-rating.vue";

export default {
  props: {
    data: {
      required: true
    }
  },
  data() {
    return {
      min: 0,
      max: 200,
      pricerange: 200
    };
  },
  computed: {
    filteredprice() {
      return this.data.filter(el => el.price < this.pricerange);
    }
  },
  components: {
    StarRating
  }
};
</script>

<style lang="scss" scoped>
.content {
  height: 100%;
  width: 100%;
}

.img-contain {
  max-height: 200px;
  display: flex;
  align-content: center;
  align-items: center;
  img {
    width: 100%;
  }
}

.img-contain :hover{
  box-shadow: gray;
}

.item {
  max-height: 500px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  margin: 20px 0;
}

aside {
  height: 100%;
  width: 100%;
}

.max {
  display: inline-block;
  float: right;
}

.card :hover{
  box-shadow: 0 6px 26px rgba(0, 0, 0, 0.1);
  -webkit-transform: scale(1.05);
          transform: scale(1.05);
}

</style>