<template>
  <div class="mainVisual">
    <img src="../assets/imgs/mainvisual.jpg" alt="" />
    <div class="text-container">
      <div class="text">
        <span class="mini-heading">MOVIE CATALOGUE</span>
        <h1>What's new?</h1>
        <a href="#movie-grid" class="button">View Movies</a>
        <div class="remark">
        <p>This app is only meant for demonstrating the basic usage of NuxtJS</p>
        <a href="http://bit.ly/3Qkl8ju">Click here to see source code on Github</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MainVisual',
}
</script>

<style lang="scss" scoped>
.mainVisual {
  height: 400px;
  position: relative;
  @media (min-width: 750px) {
    height: 500px;
  }

  &::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.6);
  }

  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .text-container {
    z-index: 99;
    position: absolute;
    top: 0;
    margin: 0;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;

    .text {
      padding: 0 16px;
      width: 100%;
      max-width: 1400px;
      margin: 0 auto;
    }

    .remark {
      margin-top: 32px;
      color: #ffffff;
      a {
        color: #ffffff;
        display: block;
      }
    }

    .mini-heading {
      font-weight: 600;
      font-size: 18px;
      text-transform: uppercase;
      color: #c92502;
      margin-bottom: 8px;
      @media (min-width: 750px) {
        font-size: 22px;
      }
    }

    h1 {
      color: #fff;
      font-size: 64px;
      font-weight: 600;
      margin-bottom: 8px;

      @media (min-width: 750px) {
        font-size: 84px;
      }

      span {
        font-weight: 500;
      }
    }

    .button {
      font-size: 20px;
      align-self: flex-start;
    }
  }
}
</style>
