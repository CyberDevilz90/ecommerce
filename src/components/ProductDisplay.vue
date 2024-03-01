<script>
export default {
  data() {
    return {
      product: null,
      currentProductId: 1,
      maxProductId: 20,
      bgColor: "",
      bgColor2: "",
      loading: false,
    };
  },
  created() {
    this.fetchProductData();
  },
  methods: {
    async fetchProductData() {
      this.loading = true;
      try {
        const response = await fetch(
          `https://fakestoreapi.com/products/${this.currentProductId}`
        );
        const data = await response.json();
        this.product = data;
        this.setBgCategory();
      } catch (error) {
        console.error("Error fetching product data:", error);
      } finally {
        this.loading = false;
      }
    },
    setBgCategory() {
      if (this.product && this.product.category) {
        switch (this.product.category.toLowerCase()) {
          case "men's clothing":
            this.bgColor = "var(--LIGHT-BLUE)";
            this.bgColor2 = "var(--DARK-BLUE)";
            break;
          case "women's clothing":
            this.bgColor = "var(--PINK)";
            this.bgColor2 = "var(--PURPLE)";
            break;
          default:
            this.bgColor = "var(--GRAY)";
            this.bgColor2 = "var(--GRAY)";
            break;
        }
      }
    },
    async getNextProduct() {
      this.currentProductId = (this.currentProductId % this.maxProductId) + 1;
      await this.fetchProductData();
    },
    getStarImagePath(category) {
      if (category === "men's clothing") {
        return require("../assets/bluedoted.png");
      } else if (category === "women's clothing") {
        return require("../assets/Ellipsed.png");
      } else {
        return null;
      }
    },
  },
  computed: {
    isCategoryUnavailable() {
      return (
        this.product &&
        !["men's clothing", "women's clothing"].includes(
          this.product.category.toLowerCase()
        )
      );
    },
  },
};
</script>

<template>
  <div v-if="loading" class="loading-screen">
    <p>Loading...</p>
  </div>
  <div v-else-if="!isCategoryUnavailable">
    <div class="bg-container" :style="{ backgroundColor: bgColor }">
      <img class="bg-pattern" src="../assets/bg-pattern.png" alt="bg-pattern" />
      <div class="content">
        <div class="image-product-container">
          <img class="image-product" v-bind:src="product.image" alt="product" />
        </div>
        <div class="product-detail-container">
          <p class="title-product" :style="{ color: bgColor2 }">
            {{ product.title }}
          </p>
          <div class="rating-container">
            <p class="category-product">{{ product.category }}</p>
            <div class="score-star">
              <p class="rating-product">2.9/5</p>
              <div>
                <img
                  class="rating-star"
                  v-bind:src="getStarImagePath(product.category.toLowerCase())"
                  alt="star-image"
                />
                <img
                  class="rating-star"
                  v-bind:src="getStarImagePath(product.category.toLowerCase())"
                  alt="star-image"
                />
                <img
                  class="rating-star"
                  v-bind:src="getStarImagePath(product.category.toLowerCase())"
                  alt="star-image"
                />
                <img
                  class="rating-star"
                  v-bind:src="getStarImagePath(product.category.toLowerCase())"
                  alt="star-image"
                />
                <img
                  class="rating-star"
                  v-bind:src="getStarImagePath(product.category.toLowerCase())"
                  alt="star-image"
                />
              </div>
            </div>
          </div>
          <div class="line-break"></div>
          <p class="description-product">
            {{ product.description }}
          </p>
          <div class="line-break"></div>
          <p class="product-price" :style="{ color: bgColor2 }">
            ${{ product.price }}
          </p>
          <div class="action-user-container">
            <div
              class="action-buy-container"
              :style="{ backgroundColor: bgColor2 }"
            >
              <p class="buy-text">Buy Now</p>
            </div>
            <div
              class="action-next-container"
              @click="getNextProduct"
              :style="{ borderColor: bgColor2 }"
            >
              <p class="next-text" :style="{ color: bgColor2 }">Next Product</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div v-else>
    <div class="bg-container" :style="{ backgroundColor: bgColor }">
      <img class="bg-pattern" src="../assets/bg-pattern.png" alt="bg-pattern" />
      <div class="content">
        <div class="product-detail-container-unavailable">
          <p class="unavailable-text">Product is Unavailable</p>
          <div
            class="action-next-container"
            @click="getNextProduct"
            :style="{ borderColor: bgColor2 }"
          >
            <p class="next-text" :style="{ color: bgColor2 }">Next Product</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
