<template>
  <div id="preloader">
    <span>l</span>
    <span>o</span>
    <span>a</span>
    <span>d</span>
    <span>i</span>
    <span>n</span>
    <span>g</span>
  </div>
</template>

<script>
export default {};
</script>

<style lang="scss">
:root {
  --effect: hover 1s linear infinite;
}

#preloader {
  text-align: center;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  span {
    display: inline-block;
    text-transform: uppercase;
    //   text-align: center;
    font-size: 4em;
    font-family: arial;
    font-weight: 600;
    transform: scale(0.5);
    color: #121212;
    -webkit-text-stroke: 2px gray;
  }

  span:nth-child(1) {
    animation: var(--effect);
  }

  span:nth-child(2) {
    animation: var(--effect) 0.125s;
  }

  span:nth-child(3) {
    animation: var(--effect) 0.25s;
  }

  span:nth-child(4) {
    animation: var(--effect) 0.375s;
  }

  span:nth-child(5) {
    animation: var(--effect) 0.5s;
  }

  span:nth-child(6) {
    animation: var(--effect) 0.675s;
  }

  span:nth-child(7) {
    animation: var(--effect) 0.75s;
  }

  @keyframes hover {
    0% {
      transform: scale(0.5);
      color: #121212;
      -webkit-text-stroke: 2px gray;
    }

    20% {
      transform: scale(1);
      color: pink;
      -webkit-text-stroke: 3px red;
      filter: drop-shadow(0 0 1px black) drop-shadow(0 0 1px black)
        drop-shadow(0 0 3px red) drop-shadow(0 0 5px red) hue-rotate(10turn);
    }

    50% {
      transform: scale(0.5);
      color: #121212;
      -webkit-text-stroke: 2px gray;
    }
  }
}
</style>