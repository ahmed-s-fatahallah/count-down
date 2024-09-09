<script setup></script>

<template>
  <section class="wrapper">
    <h1 class="header">Draft <span>CountDown</span></h1>
    <section class="counter">
      <p>Hours</p>
      <p>Minutes</p>
      <p>Seconds</p>
      <div><span>2</span><span>1</span></div>
      <div><span>0</span><span>0</span></div>
      <div><span>0</span><span>0</span></div>
    </section>
  </section>
</template>

<style scoped>
.header {
  text-align: center;
  font-weight: 100;
  text-transform: uppercase;
}
.header > span {
  color: rgb(255, 0, 0, 0.8);
}

.counter {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  column-gap: 1.6rem;
  row-gap: 1rem;
  justify-items: center;
  text-transform: uppercase;
  font-weight: 400;
}
.counter p {
  font-size: 1.6rem;
}

.counter span {
  font-family: serif;
  display: inline-block;
  box-shadow: 1px 2px 6px 1px rgba(0, 0, 0, 0.2);
  font-size: 80px;
  padding: 1rem 2rem;
  border-radius: 0.5rem;
  color: rgb(255, 0, 0, 0.8);
  position: relative;
  font-weight: 600;
  line-height: 1;
  perspective: 500px;
}
.counter > div > span:first-child {
  margin-right: 1rem;
}

.counter > div > span::before {
  width: 100%;
  height: 50%;

  display: block;
  content: var(--before-content);
  position: absolute;
  top: 0;
  left: 0;
  background-color: white;
  border-radius: 0.5rem;
  text-align: center;
  overflow: hidden;
  line-height: 1.25;
}

.counter > div > span:hover::before {
  transform-origin: bottom;

  animation: flip 400ms ease-out forwards;
}

@keyframes flip {
  0% {
    transform: rotateX(0deg);
  }
  50% {
    transform: rotateX(-90deg);
    z-index: 10;
  }
  100% {
    transform: rotateX(-180deg);
    z-index: 10;
  }
}
</style>
