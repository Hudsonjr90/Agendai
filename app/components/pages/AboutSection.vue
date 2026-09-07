<script setup lang="ts">
import profileImage from '~/assets/images/profile/my.webp'

const { data: portfolio, pending, error } = usePortfolio()
const { isMobile } = useMobile()

const principles = [
  {
    title: 'Arquitetura & Escalabilidade',
    description:
      'Estruturo soluções pensando em crescimento, manutenção e evolução contínua do produto.',
    icon: 'mdi-layers-outline',
  },
  {
    title: 'Performance & Qualidade',
    description:
      'Busco código eficiente, consultas otimizadas e experiências rápidas e consistentes.',
    icon: 'mdi-speedometer',
  },
  {
    title: 'Segurança & Boas Práticas',
    description:
      'Aplico princípios de segurança, organização e padrões que tornam o software mais confiável.',
    icon: 'mdi-shield-check-outline',
  },
]
</script>

<template>
  <section
    id="sobre"
    class="q-py-xl"
    aria-labelledby="about-title"
  >
    <div class="wrapper">
      <div
        v-if="pending"
        class="row items-center justify-center q-py-xl"
        aria-live="polite"
        aria-busy="true"
      >
        <q-spinner
          color="primary"
          size="50px"
          aria-label="Carregando informações do perfil"
        />
      </div>

      <div
        v-else-if="error"
        class="row items-center justify-center q-py-xl"
        role="alert"
      >
        <q-banner
          rounded
          class="bg-negative text-white"
        >
          Não foi possível carregar os dados do portfólio.
        </q-banner>
      </div>

      <template v-else-if="portfolio">
        <!-- Apresentação principal -->
        <div class="row items-center q-col-gutter-xl">
          <div class="col-12 col-md-7">
            <h1
              id="about-title"
              class="text-weight-bold q-mb-md"
              :class="isMobile ? 'text-h4' : 'text-h2'"
            >
              {{ portfolio.profile.name }}
            </h1>

            <p
              class="text-primary text-weight-medium q-mb-lg"
              :class="isMobile ? 'text-h5' : 'text-h4'"
            >
              {{ portfolio.profile.headline }}
            </p>

            <p class="text-body1 q-mb-xl">
              {{ portfolio.profile.shortBio }}
            </p>

            <div class="row items-center q-gutter-sm">
              <q-btn-dropdown
                outline
                icon="mdi-file-document-outline"
                label="Meu currículo"
                no-caps
                aria-label="Abrir opções de currículo"
              >
                <q-list class="bg-primary text-white">
                  <q-item
                    clickable
                    v-close-popup
                    tag="a"
                    href="/documents/HudsonKennedy.pdf"
                    download="HudsonKennedy.pdf"
                    aria-label="Baixar currículo criativo em PDF"
                  >
                    <q-item-section avatar>
                      <q-icon
                        name="mdi-lightbulb-on-outline"
                        aria-hidden="true"
                      />
                    </q-item-section>

                    <q-item-section>
                      <q-item-label>
                        Currículo Criativo
                      </q-item-label>
                    </q-item-section>
                  </q-item>

                  <q-separator dark />

                  <q-item
                    clickable
                    v-close-popup
                    tag="a"
                    href="/documents/HudsonKennedyAts.pdf"
                    download="HudsonKennedyAts.pdf"
                    aria-label="Baixar currículo ATS em PDF"
                  >
                    <q-item-section avatar>
                      <q-icon
                        name="mdi-robot-outline"
                        aria-hidden="true"
                      />
                    </q-item-section>

                    <q-item-section>
                      <q-item-label>
                        Currículo ATS
                      </q-item-label>
                    </q-item-section>
                  </q-item>
                </q-list>
              </q-btn-dropdown>
            </div>
          </div>

          <!-- Avatar com efeito elétrico -->
          <div class="col-12 col-md-5 flex flex-center">
            <div class="profile-avatar">
              <!-- Anel elétrico -->
              <div
                class="profile-avatar__electric"
                aria-hidden="true"
              >
                <span class="electric-spark electric-spark--1"></span>
                <span class="electric-spark electric-spark--2"></span>
                <span class="electric-spark electric-spark--3"></span>
                <span class="electric-spark electric-spark--4"></span>
                <span class="electric-spark electric-spark--5"></span>
                <span class="electric-spark electric-spark--6"></span>
              </div>

              <q-avatar
                :size="isMobile ? '200px' : '280px'"
                class="profile-avatar__image shadow-10"
              >
                <img
                  :src="profileImage"
                  :alt="`${portfolio.profile.name} - ${portfolio.profile.headline}`"
                  loading="eager"
                />
              </q-avatar>
            </div>
          </div>
        </div>

        <!-- Sobre mim / Como eu trabalho -->
        <div class="row items-center q-col-gutter-xl">
          <div class="col-12 col-md-7">
            <div
              class="text-overline text-primary"
              aria-hidden="true"
            >
              Sobre mim
            </div>

            <h2
              class="text-weight-bold q-mb-lg"
              :class="isMobile ? 'text-h4' : 'text-h3'"
            >
              Experiência que gera resultado
            </h2>

            <p class="text-body1">
              {{ portfolio.profile.about }}
            </p>
          </div>

          <div class="col-12 col-md-5">
            <div
              class="text-overline text-primary"
              aria-hidden="true"
            >
              Como eu trabalho
            </div>

            <h2
              class="text-weight-bold q-mb-md"
              :class="isMobile ? 'text-h5' : 'text-h4'"
            >
              Engenharia com propósito
            </h2>

            <div
              class="column q-gutter-md"
              aria-label="Princípios de trabalho"
            >
              <q-card
                v-for="principle in principles"
                :key="principle.title"
                flat
                bordered
                class="bg-primary"
              >
                <q-card-section
                  class="row items-center no-wrap q-gutter-md"
                >
                  <q-avatar
                    color="primary"
                    text-color="white"
                    size="52px"
                  >
                    <q-icon
                      :name="principle.icon"
                      size="28px"
                      aria-hidden="true"
                    />
                  </q-avatar>

                  <div class="col">
                    <h3
                      class="text-subtitle1 text-white text-weight-bold"
                    >
                      {{ principle.title }}
                    </h3>

                    <p
                      class="text-body2 text-white q-mt-xs q-mb-none"
                    >
                      {{ principle.description }}
                    </p>
                  </div>
                </q-card-section>
              </q-card>
            </div>
          </div>
        </div>
      </template>
    </div>
  </section>
</template>

<style scoped>
/* =========================================================
   AVATAR / EFEITO ELÉTRICO
   ========================================================= */

.profile-avatar {
  --avatar-size: 280px;

  position: relative;

  width: var(--avatar-size);
  height: var(--avatar-size);

  display: flex;
  align-items: center;
  justify-content: center;
}

/* =========================================================
   IMAGEM
   ========================================================= */

.profile-avatar__image {
  position: relative;

  z-index: 3;

  transition:
    transform 0.4s ease,
    box-shadow 0.4s ease;
}

/* =========================================================
   CONTAINER DO EFEITO
   ========================================================= */

.profile-avatar__electric {
  position: absolute;

  inset: -18px;

  z-index: 2;

  border-radius: 50%;

  opacity: 0;

  transform: scale(0.9);

  pointer-events: none;

  transition:
    opacity 0.25s ease,
    transform 0.35s ease;
}

/* =========================================================
   ANEL ELÉTRICO PRINCIPAL
   ========================================================= */

.profile-avatar__electric::before {
  content: '';

  position: absolute;

  inset: 0;

  border-radius: 50%;

  background:
    conic-gradient(
      from 0deg,

      transparent 0deg,
      transparent 12deg,

      rgba(0, 174, 239, 0.95) 16deg,
      rgba(0, 229, 255, 1) 22deg,
      transparent 29deg,

      transparent 55deg,

      rgba(0, 174, 239, 0.9) 62deg,
      rgba(0, 229, 255, 1) 69deg,
      transparent 77deg,

      transparent 118deg,

      rgba(0, 174, 239, 0.9) 126deg,
      rgba(0, 229, 255, 1) 133deg,
      transparent 141deg,

      transparent 190deg,

      rgba(0, 174, 239, 0.95) 198deg,
      rgba(0, 229, 255, 1) 206deg,
      transparent 214deg,

      transparent 255deg,

      rgba(0, 174, 239, 0.9) 264deg,
      rgba(0, 229, 255, 1) 271deg,
      transparent 280deg,

      transparent 320deg,

      rgba(0, 174, 239, 0.95) 328deg,
      rgba(0, 229, 255, 1) 335deg,
      transparent 344deg,

      transparent 360deg
    );

  -webkit-mask:
    radial-gradient(
      farthest-side,
      transparent calc(100% - 4px),
      #000 calc(100% - 3px)
    );

  mask:
    radial-gradient(
      farthest-side,
      transparent calc(100% - 4px),
      #000 calc(100% - 3px)
    );

  filter:
    drop-shadow(0 0 4px rgba(0, 229, 255, 0.95))
    drop-shadow(0 0 10px rgba(0, 174, 239, 0.75))
    drop-shadow(0 0 18px rgba(0, 174, 239, 0.35));

  transform-origin: center center;

  /*
   * A animação só é ativada no hover.
   */
  animation: none;
}

/* =========================================================
   SEGUNDO ANEL
   ========================================================= */

.profile-avatar__electric::after {
  content: '';

  position: absolute;

  inset: 7px;

  border-radius: 50%;

  border: 1px solid rgba(0, 229, 255, 0.35);

  box-shadow:
    0 0 8px rgba(0, 229, 255, 0.5),
    inset 0 0 8px rgba(0, 229, 255, 0.25);

  animation: none;
}

/* =========================================================
   FAÍSCAS
   ========================================================= */

.electric-spark {
  position: absolute;

  width: 5px;
  height: 5px;

  border-radius: 50%;

  background: #00e5ff;

  box-shadow:
    0 0 5px #00e5ff,
    0 0 12px rgba(0, 174, 239, 0.9),
    0 0 20px rgba(0, 174, 239, 0.55);

  opacity: 0;

  transform: scale(0);

  animation: none;
}

/* =========================================================
   POSIÇÕES DAS FAÍSCAS
   ========================================================= */

.electric-spark--1 {
  top: 5%;
  left: 25%;

  animation-delay: 0s;
}

.electric-spark--2 {
  top: 18%;
  right: 3%;

  animation-delay: 0.3s;
}

.electric-spark--3 {
  bottom: 20%;
  right: 0;

  animation-delay: 0.6s;
}

.electric-spark--4 {
  bottom: 4%;
  left: 30%;

  animation-delay: 0.9s;
}

.electric-spark--5 {
  bottom: 27%;
  left: 0;

  animation-delay: 1.2s;
}

.electric-spark--6 {
  top: 23%;
  left: 5%;

  animation-delay: 1.5s;
}

/* =========================================================
   HOVER
   ========================================================= */

.profile-avatar:hover .profile-avatar__electric {
  opacity: 1;

  transform: scale(1);
}

.profile-avatar:hover .profile-avatar__electric::before {
  animation: electric-rotate 4.5s linear infinite;
}

.profile-avatar:hover .profile-avatar__electric::after {
  animation: electric-pulse 1.4s ease-in-out infinite;
}

.profile-avatar:hover .profile-avatar__image {
  transform: scale(1.015);

  box-shadow:
    0 0 15px rgba(0, 174, 239, 0.45),
    0 0 35px rgba(0, 174, 239, 0.25);
}

.profile-avatar:hover .electric-spark {
  opacity: 1;

  animation: electric-spark 1.8s ease-in-out infinite;
}

/* =========================================================
   ANIMAÇÕES
   ========================================================= */

/*
 * Rotação do anel elétrico
 */
@keyframes electric-rotate {
  from {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(360deg);
  }
}

/*
 * Pulsação do segundo anel
 */
@keyframes electric-pulse {
  0%,
  100% {
    opacity: 0.35;

    transform: scale(0.98);
  }

  50% {
    opacity: 0.9;

    transform: scale(1.015);
  }
}

/*
 * Aparição das faíscas
 */
@keyframes electric-spark {
  0%,
  100% {
    opacity: 0;

    transform: scale(0);
  }

  20% {
    opacity: 1;

    transform: scale(1.3);
  }

  45% {
    opacity: 0.6;

    transform: scale(0.8);
  }

  70% {
    opacity: 0;

    transform: scale(0);
  }
}

/* =========================================================
   MOBILE
   ========================================================= */

@media (max-width: 600px) {
  .profile-avatar {
    --avatar-size: 200px;
  }

   .profile-avatar__electric {
    inset: -14px;
    opacity: 1;
    transform: scale(1);
  }

  .profile-avatar__electric::before {
    animation: electric-rotate 4.5s linear infinite;
  }

  .profile-avatar__electric::after {
    animation: electric-pulse 1.4s ease-in-out infinite;
  }

  .profile-avatar__image {
    box-shadow:
      0 0 15px rgba(0, 174, 239, 0.45),
      0 0 35px rgba(0, 174, 239, 0.25);
  }

  .electric-spark {
    animation: electric-spark 1.8s ease-in-out infinite;
  }
}

/* =========================================================
   ACESSIBILIDADE
   ========================================================= */

@media (prefers-reduced-motion: reduce) {
  .profile-avatar__electric::before,
  .profile-avatar__electric::after,
  .electric-spark {
    animation: none;
  }

  .profile-avatar:hover .profile-avatar__electric {
    opacity: 1;
  }
}
</style>