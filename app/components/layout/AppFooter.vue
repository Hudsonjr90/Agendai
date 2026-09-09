<template>
  <footer class="footer">
    <!-- <svg
      viewBox="0 0 120 28"
      preserveAspectRatio="none"
    >
      <defs>
        <filter id="goo">
          <feGaussianBlur
            in="SourceGraphic"
            stdDeviation="1"
            result="blur"
          />

          <feColorMatrix
            in="blur"
            mode="matrix"
            values="
              1 0 0 0 0
              0 1 0 0 0
              0 0 1 0 0
              0 0 0 13 -9
            "
            result="goo"
          />

          <feBlend
            in="SourceGraphic"
            in2="goo"
          />
        </filter>

        <path
          id="wave"
          d="M 0,10
            C 30,10 30,15 60,15
            90,15 90,10 120,10
            150,10 150,15 180,15
            210,15 210,10 240,10
            v 28
            h -240
            z"
        />
      </defs>

      <use
        id="wave3"
        class="wave"
        href="#wave"
        x="0"
        y="-2"
      />

      <use
        id="wave2"
        class="wave"
        href="#wave"
        x="0"
        y="0"
      />

      <g
        class="gooeff"
        filter="url(#goo)"
      >
        <circle class="drop drop1" cx="20" cy="2" r="8.8" />
        <circle class="drop drop2" cx="25" cy="2.5" r="7.5" />
        <circle class="drop drop3" cx="16" cy="2.8" r="9.2" />

        <circle class="drop drop4" cx="18" cy="2" r="8.8" />
        <circle class="drop drop5" cx="22" cy="2.5" r="7.5" />
        <circle class="drop drop6" cx="26" cy="2.8" r="9.2" />

        <circle class="drop drop1" cx="5" cy="4.4" r="8.8" />
        <circle class="drop drop2" cx="5" cy="4.1" r="7.5" />
        <circle class="drop drop3" cx="8" cy="3.8" r="9.2" />

        <circle class="drop drop4" cx="3" cy="4.4" r="8.8" />
        <circle class="drop drop5" cx="7" cy="4.1" r="7.5" />
        <circle class="drop drop6" cx="10" cy="4.3" r="9.2" />

        <circle class="drop drop1" cx="1.2" cy="5.4" r="8.8" />
        <circle class="drop drop2" cx="5.2" cy="5.1" r="7.5" />
        <circle class="drop drop3" cx="10.2" cy="5.3" r="9.2" />

        <circle class="drop drop4" cx="3.2" cy="5.4" r="8.8" />
        <circle class="drop drop5" cx="14.2" cy="5.1" r="7.5" />
        <circle class="drop drop6" cx="17.2" cy="4.8" r="9.2" />

        <use
          id="wave1"
          class="wave"
          href="#wave"
          x="0"
          y="1"
        />
      </g>
    </svg> -->

   <div class="waves">
		<div class="wave" id="wave1"></div>
		<div class="wave" id="wave2"></div>
		<div class="wave" id="wave3"></div>
		<div class="wave" id="wave4"></div>
	</div>
    <div class="footer-content">
      <div class="text-center text-white" :class="isMobile ? '' : 'text-h6'">
        © 2012 - {{ new Date().getFullYear() }} Hudson Kennedy.
        Todos os direitos reservados.
      </div>
    </div>
  </footer>
</template>

<script setup lang="ts">
const { isMobile } = useMobile()
</script>

<style scoped>
.footer {
  position: relative;
  background: #00A8E8;
  min-height: 100px;
  padding: 20px 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  margin-top: 1.3%;
}

.wave {
  position: absolute;
  top: -70px;
  left: 0;
  width: 100%;
  height: 100px;
  background: url("/waves.png");
  background-size: 1000px 100px;
}

.wave#wave1 {
  z-index: 1000;
  opacity: 1;
  bottom: 0;
  animation: animateWaves 4s linear infinite;
}

.wave#wave2 {
  z-index: 999;
  opacity: 0.5;
  bottom: 10px;
  animation: animate 4s linear infinite !important;
}

.wave#wave3 {
  z-index: 1000;
  opacity: 0.2;
  bottom: 15px;
  animation: animateWaves 3s linear infinite;
}

.wave#wave4 {
  z-index: 999;
  opacity: 0.7;
  bottom: 20px;
  animation: animate 3s linear infinite;
}

@keyframes animateWaves {
  0% {
    background-position-x: 1000px;
  }
  100% {
    background-position-x: 0px;
  }
}

@keyframes animate {
  0% {
    background-position-x: -1000px;
  }
  100% {
    background-position-x: 0px;
  }
}


@media screen and (max-width: 768px) {
  .footer {
    padding: 20px 20px;
    margin-top: 5%;
  }
}
</style>