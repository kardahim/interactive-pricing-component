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
    <!-- slider -->
    <div class="card__toogle"></div>
    <div class="card__divider"></div>
    <div class="card__footer"></div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "App",
  data() {
    return {
      pageViews: 100,
      price: 16,
      isYearly: false,
    };
  },
});
</script>

<style lang="scss">
@import "./assets/styles/variables.scss";

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
  }
}
</style>
