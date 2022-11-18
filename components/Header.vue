<template>
  <div class="header-component">
    <div class="header-title">
      <h1>Social Media</h1>
      <hr />
    </div>
    <div class="header-box">
      <h2>Get Connected</h2>
      <h3>Add your social media URLs to show them on your Kangaroo page.</h3>
      <img src="~/static/icons/main-graphic.svg" alt="" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "@nuxtjs/composition-api";
export default defineComponent({});
</script>

<style lang="scss" scoped>
.header-component {
  .header-title {
    width: fit-content;
    margin-bottom: 2rem;
    h1 {
      color: $main-color;
      @media screen and (max-width: 500px) {
        font-size: 1.7rem;
      }
    }
    hr {
      width: 85%;
      border: none;
      border-bottom: 3px solid $main-color;
      margin: 0 auto;
    }
  }

  .header-box {
    width: 100%;
    background-color: $main-light-color;
    position: relative;
    padding: 3rem 2rem;
    border-radius: 2rem;
    display: flex;
    flex-direction: column;
    h2 {
      color: $main-color;
      font-weight: 400;
      margin-bottom: 1rem;
      font-size: 2rem;
    }
    h3 {
      color: $grey;
      font-weight: 400;
    }
    img {
      position: absolute;
      bottom: 3rem;
      right: 3rem;
      width: 15vw;
      max-width: 12rem;
    }
    @media screen and (max-width: 900px) {
      align-items: center;
      text-align: center;
      img {
        position: relative;
        bottom: unset;
        right: unset;
        margin-top: 1rem;
      }
    }
    @media screen and (max-width: 500px) {
      h2 {
        font-size: 1.6rem;
      }
      h3 {
        font-size: 1rem;
      }
    }
  }
}
</style>
