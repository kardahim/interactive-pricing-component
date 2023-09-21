<template>
  <img src="./assets/images/bg-pattern.svg" alt="pattern" class="pattern" />
  <div class="header">
    <h1 class="header__main">Simple, traffic-based pricing</h1>
    <p class="header__sub">
      Sign-up for our 30-day trial. No credit card required.
    </p>
    <img src="./assets/images/pattern-circles.svg" alt="circles pattern" />
  </div>
  <div class="card">
    <div class="card__info">
      <div class="card__info__views">{{ pageViews }}k pageviews</div>
      <div class="card__info__price">
        <span>${{ price.toFixed(2) }}</span>
        <span>{{ isYearly ? "year" : "month" }}</span>
      </div>
    </div>
    <input
      class="card__range"
      type="range"
      v-model="pageViews"
      max="200"
      min="10"
      step="10"
      @input="updateSliderBackground"
    />
    <div class="card__toogle">
      <div class="card__toogle__monthly">Monthly Billing</div>
      <input type="checkbox" id="switch" v-model="isYearly" />
      <label for="switch">Toogle</label>
      <div class="card__toogle__yearly">
        <span>Yearly Billing</span>
        <span>25% discount</span>
      </div>
    </div>
    <div class="card__divider"></div>
    <div class="card__footer"></div>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, onMounted, ref } from "vue";

export default defineComponent({
  name: "App",
  setup() {
    const pageViews = ref(100);
    const isYearly = ref(false);
    const price = computed(() => {
      let tempPrice;

      if (isYearly.value) {
        tempPrice = (pageViews.value / 100) * 16 * 12;
        tempPrice *= 0.75;
      } else {
        tempPrice = (pageViews.value / 100) * 16;
      }

      return tempPrice;
    });

    const updateSliderBackground = () => {
      const sliderElement = document.querySelector(
        ".card__range"
      ) as HTMLInputElement;
      const sliderValue: number = parseInt(sliderElement.value);

      const progress =
        ((sliderValue - parseInt(sliderElement.min)) /
          (parseInt(sliderElement.max) - parseInt(sliderElement.min))) *
        100;

      sliderElement.style.background = `linear-gradient(to right, hsl(174, 77%, 80%) ${progress}%, hsl(224, 65%, 95%) ${progress}%)`;
    };

    onMounted(() => {
      updateSliderBackground();
    });

    return { pageViews, price, isYearly, updateSliderBackground };
  },
});
</script>

<style lang="scss">
@import "./assets/styles/variables.scss";
@import "./assets/styles/mixins.scss";

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;

  position: relative;
  background: $main-background-color;

  font-family: $ff;

  .pattern {
    position: absolute;
    top: -15%;
    left: 0;
    z-index: 1;
    width: 100%;
  }

  .header {
    position: relative;
    text-align: center;
    z-index: 2;

    margin-top: 110px;
    margin-bottom: 95px;

    &__main {
      color: $cta-background-color;
    }

    &__sub {
      color: $text-color;
    }

    img {
      position: absolute;
      margin: auto;
      left: 0;
      right: 0;
      top: 0;
      bottom: 0;
      z-index: -1;
    }
  }

  .card {
    z-index: 2;
    width: 544px;
    height: 400px;
    background: $pricing-component-background-color;
    border-radius: 15px;
    padding: 52px 43px 40px;

    // TODO: add shadow

    &__info {
      display: flex;
      justify-content: space-between;
      align-items: center;

      &__views {
        color: $text-color;
        text-transform: uppercase;
        font-size: $fs-paragraph;
        font-weight: $fw-extra-bold;
        letter-spacing: 1px;
      }

      &__price {
        span:nth-child(1) {
          color: $cta-background-color;
          font-size: 40px;
          font-weight: $fw-extra-bold;
          vertical-align: middle;
        }

        span:nth-child(2) {
          vertical-align: middle;
          color: $text-color;
          margin-left: 5px;
          &::before {
            content: "/ ";
          }
        }
      }
    }

    &__range {
      margin-top: 38px;

      -webkit-appearance: none;
      appearance: none;
      width: 100%;
      cursor: pointer;
      outline: none;
      border-radius: 15px;

      height: 6px;
      background: $empty-slider-bar-color;

      // TODO: change effect
      // thumb: Webkit
      &::-webkit-slider-thumb {
        -webkit-appearance: none;
        appearance: none;
        @include slider-thumb;
      }

      // thumb: Firefox
      &::-moz-range-thumb {
        @include slider-thumb;
      }

      // active effects: Webkit
      &::-webkit-slider-thumb:hover {
        box-shadow: 0 0 0 10px hsla(174, 77%, 80%, 0.1);
      }

      &:active::-webkit-slider-thumb {
        box-shadow: 0 0 0 13px hsla(174, 77%, 80%, 0.2);
      }

      &:focus::-webkit-slider-thumb {
        box-shadow: 0 0 0 13px hsla(174, 77%, 80%, 0.2);
      }

      // active effects: Firefox
      &::-moz-range-thumb:hover {
        box-shadow: 0 0 0 10px hsla(174, 77%, 80%, 0.1);
      }

      &:active::-moz-range-thumb {
        box-shadow: 0 0 0 13px hsla(174, 77%, 80%, 0.2);
      }

      &:focus::-moz-range-thumb {
        box-shadow: 0 0 0 13px hsla(174, 77%, 80%, 0.2);
      }
    }

    &__toogle {
      display: flex;
      justify-content: right;
      align-items: center;
      margin-top: 50px;
      font-size: 14px;
      color: $text-color;

      &__yearly {
        span:nth-child(1) {
        }
        span:nth-child(2) {
          margin: 0 15px 10px 0;
          color: hsl(15, 100%, 70%);
          margin-left: 5px;
          background: hsla(15, 100%, 70%, 0.1);
          border-radius: 20px;
          padding: 0 5px;
        }
      }

      input[type="checkbox"] {
        height: 0;
        width: 0;
        visibility: hidden;
      }

      // toogle container
      label {
        margin: 0 12px;
        cursor: pointer;
        text-indent: -9999px;
        width: 44px;
        height: 24px;
        background: $toogle-background-color;
        display: block;
        border-radius: 12px;
        position: relative;
      }

      // toogle circle
      label:after {
        content: "";
        position: absolute;
        top: 3px;
        left: 4px;
        width: 18px;
        height: 18px;
        background: #fff;
        border-radius: 50%;
        transition: 0.3s;
      }

      input:checked + label {
        background: $slider-background-color;
        transition: 0.3s;
      }
      label:hover,
      input:checked + label:hover {
        background: $slider-bar-color;
        transition: 0.3s;
      }

      input:checked + label:after {
        left: calc(100% - 23px);
      }
    }
  }
}
</style>
