<script setup lang="ts">
import profileImage from '~/assets/images/profile/home.webp'

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

          <div class="col-12 col-md-5 flex flex-center">
            <q-avatar
              :size="isMobile ? '200px' : '280px'"
              class="shadow-10"
            >
              <img
                :src="profileImage"
                :alt="`${portfolio.profile.name} - ${portfolio.profile.headline}`"
                loading="eager"
              />
            </q-avatar>
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