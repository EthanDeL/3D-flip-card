<template>
  <div class="page">
    <h1>3D Flip Card with GSAP</h1>

    <section ref="section" class="section">
      <div class="card-wrapper">
        <div ref="card" class="card">

          <div class="card__face card__front">
            Front
          </div>

          <div class="card__face card__back">
            Back
          </div>

        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

const card = ref(null);
const section = ref(null);

let ctx;

onMounted(() => {
  ctx = gsap.context(() => {

    const tl = gsap.timeline({
      scrollTrigger: {
        trigger: section.value,
        start: "top top",
        end: "+=800",
        scrub: 1.2,
        pin: true,
      }
    });

    tl.fromTo(
      card.value,
      { scale: 0.5 },
      { scale: 1, ease: "none", duration: 1 }
    )
    .to(card.value, {
      rotateY: 180,
      rotationX: 360,
      ease: "none",
      duration: 2
    });

  }, section.value);
});

onUnmounted(() => {
  ctx?.revert();
});
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  text-decoration: none;
  list-style: none;
}

.page {
  width: 100%;
  min-height: 300vh;
  padding-top: 50px;
  text-align: center;
  background: hsl(240, 2%, 9%);
  color: hsl(0, 6%, 97%);
  font-family: sans-serif;
}

.section {
  min-height: 100vh;
  display: grid;
  place-items: center;
}

.card-wrapper {
  perspective: 1600px;
  width: 360px;
  height: 230px;
}

.card {
  width: 100%;
  height: 100%;
  position: relative;
  transform-style: preserve-3d;
  border-radius: 24px;
}

.card__face {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 24px;
  border: 1px solid hsla(0, 0%, 100%, 0.15);
  backface-visibility: hidden;
  color: hsl(0, 6%, 97%);
  font-size: 2rem;
  font-weight: 700;
  letter-spacing: .5em;
  box-shadow:
    0 30px 60px rgba(0,0,0,.45),
    inset 0 1px 1px hsla(0, 0%, 100%, 0.15);
  backdrop-filter: blur(20px);
  transition: box-shadow .3s ease;
  overflow: hidden;
}

/* Reflet */
.card__face::before{
  content:"";
  position:absolute;
  inset:0;
  background:
    linear-gradient(
      135deg,
      hsla(0, 0%, 100%, 0.28) 0%,
      hsla(0, 0%, 100%, 0.05) 30%,
      transparent 60%
    );
}

/* Halo */
.card__face::after{
  content:"";
  position:absolute;
  width:250px;
  height:250px;
  top:-120px;
  right:-80px;
  border-radius:50%;
  background:hsla(0, 0%, 100%, 0.15);
  filter:blur(70px);
}

.card__back{
  background:
    radial-gradient(circle at top left,hsl(221, 100%, 75%) 0%,transparent 40%),
    linear-gradient(135deg,hsl(225, 99%, 57%),hsl(225, 100%, 65%),hsl(251, 100%, 68%));
}

.card__front{
  transform:rotateY(180deg);

  background:
    radial-gradient(circle at bottom right,hsl(19, 100%, 77%) 0%,transparent 35%),
    linear-gradient(135deg,hsl(14, 100%, 62%),hsl(16, 100%, 66%),hsl(342, 100%, 62%));
}
</style>