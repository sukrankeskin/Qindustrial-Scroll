<template>
  <div class="container">
    <section class="hero"></section>
    <section class="services">
      <div class="services-header">
        <div class="col"></div>
        <div class="col"><h1>All Services</h1></div>
      </div>
      <div class="service">
        <div class="service-info">
          <h1>Custom Web Development</h1>
          <p>
            We provide bespoke web development solutions tailored to your
            business needs. Our team ensures top-notch performance and
            scalability.
          </p>
        </div>
        <div class="service-img">
          <div class="img">
            <img src="../assets/img1.jpg" alt="Web Development" />
          </div>
        </div>
      </div>
      <div class="service">
        <div class="service-info">
          <h1>Mobile App Development</h1>
          <p>
            Crafting intuitive and engaging mobile applications for both Android
            and iOS platforms. Enhance your user experience with our expert
            team.
          </p>
        </div>
        <div class="service-img">
          <div class="img">
            <img src="../assets/img2.jpg" alt="App Development" />
          </div>
        </div>
      </div>
      <div class="service">
        <div class="service-info">
          <h1>Digital Marketing</h1>
          <p>
            Comprehensive digital marketing services to boost your online
            presence. From SEO to social media campaigns, we cover it all.
          </p>
        </div>
        <div class="service-img">
          <div class="img">
            <img src="../assets/img3.jpg" alt="Digital Marketing" />
          </div>
        </div>
      </div>
      <div class="service">
        <div class="service-info">
          <h1>Cloud Solutions</h1>
          <p>
            Reliable and secure cloud solutions to streamline your business
            operations. Leverage the power of the cloud with our expertise.
          </p>
        </div>
        <div class="service-img">
          <div class="img">
            <img src="../assets/img4.jpg" alt="Cloud Solutions" />
          </div>
        </div>
      </div>
      <div class="service">
        <div class="service-info">
          <h1>IT Consultancy</h1>
          <p>
            Expert IT consultancy services to guide your business through
            digital transformation. Optimize your IT infrastructure with our
            insights.
          </p>
        </div>
        <div class="service-img">
          <div class="img">
            <img src="../assets/img5.jpg" alt="IT Consultancy" />
          </div>
        </div>
      </div>
    </section>

    <section class="footer"></section>
  </div>
</template>

<script>
import { onMounted } from "vue";
import gsap from "gsap";
import ScrollTrigger from "gsap/ScrollTrigger";
import Lenis from "@studio-freight/lenis";

gsap.registerPlugin(ScrollTrigger);

export default {
  mounted() {
    const lenis = new Lenis();

    lenis.on("scroll", (e) => {
      console.log(e);
    });

    lenis.on("scroll", ScrollTrigger.update);

    gsap.ticker.add((time) => {
      lenis.raf(time * 1000);
    });

    gsap.ticker.lagSmoothing(0);

    const services = gsap.utils.toArray(".service");

    const observerOptions = {
      root: null,
      rootMargin: "0px",
      threshold: 0.1,
    };

    const observerCallback = (entries, observer) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          const service = entry.target;
          const imgContainer = service.querySelector(".img");

          ScrollTrigger.create({
            trigger: service,
            start: "bottom bottom",
            end: "top top",
            scrub: true,
            onUpdate: (self) => {
              let progress = self.progress;
              let newWidth = 30 + 70 * progress;
              gsap.to(imgContainer, {
                width: newWidth + "%",
                duration: 0.1,
                ease: "none",
              });
            },
          });

          ScrollTrigger.create({
            trigger: service,
            start: "top bottom",
            end: "top top",
            scrub: true,
            onUpdate: (self) => {
              let progress = self.progress;
              let newHeight = 150 + 300 * progress;
              gsap.to(service, {
                height: newHeight + "px",
                duration: 0.1,
                ease: "none",
              });
            },
          });

          observer.unobserve(service);
        }
      });
    };

    const observer = new IntersectionObserver(
      observerCallback,
      observerOptions
    );

    services.forEach((service) => {
      observer.observe(service);
    });
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html,
body {
  font-family: "PP Neue Montreal";
}

.container {
  width: 100%;
  height: 100%;
}

h1 {
  color: #fff;
  font-size: 36px;
  font-weight: 500;
}

p {
  color: #fff;
  font-size: 15px;
  font-weight: 400;
  line-height: 150%;
}

img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.hero {
  width: 100vw;
  height: 100vh;
  background: url(../assets/hero.jpg) no-repeat 50% 50%;
  background-size: cover;
  padding: 2em;
}

.footer {
  width: 100%;
  height: 100vh;
  background: url(../assets/footer.jpg) no-repeat 50% 50%;
  background-size: cover;
}

.services {
  background: #000;
  padding: 8em 2em;
  display: flex;
  flex-direction: column;
}

.services-header {
  width: 100%;
  display: flex;
  gap: 4em;
}

.services-header .col:nth-child(1) {
  flex: 2;
}

.services-header .col:nth-child(2) {
  flex: 5;
  padding: 1em;
}

.service {
  display: flex;
  gap: 2em;
  height: 150px;
  border-top: 1px solid rgba(255, 255, 255, 0.2);
}

.service-info {
  flex: 2;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 1em;
}

.service-img {
  flex: 5;
  width: 100%;
  height: 100%;
  padding: 1em;
}

.img {
  width: 30%;
  height: 100%;
  border-radius: 10px;
  overflow: hidden;
}

/* lenis scroll */
html.lenis,
html.lenis body {
  height: 500vh;
}

.lenis.lenis-smooth {
  scroll-behavior: auto !important;
}

.lenis.lenis-smooth [data-lenis-prevent] {
  overscroll-behavior: contain;
}

.lenis.lenis-stopped {
  overflow: hidden;
}

.lenis.lenis-smooth iframe {
  pointer-events: none;
}
</style>
