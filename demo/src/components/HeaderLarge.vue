<script setup lang="ts">
import StatsBar from "@/components/StatsBar.vue";
import NavigationBar from "@/components/menu/NavigationBar.vue";
import Logo from "@/components/logos/LogoMain.vue";
import { ArrowDownIcon } from '@heroicons/vue/24/solid';
import HeroImage from '@i/cal.webp';
import OvalLight from '@i/oval-light.webp';
import { RouterLink } from "vue-router";
import { useI18n } from 'vue-i18n';
const { t, locale } = useI18n();
</script>

<template>
  <div class="w-full bg-gradient-to-tr from-primary via-primary to-primary-light py-5 shadow-lg dark:from-primary dark:via-primary-dark dark:to-primary-dark">
    <div class="container flex justify-end py-3 xs:justify-between xs:py-0">
      <div class="-mt-1 hidden w-auto max-w-[150px] flex-grow xs:block">
        <Logo class="force-light" />
      </div>
      <NavigationBar />
    </div>
    <div class="container m-auto mb-16 mt-8 grid grid-cols-1 justify-items-center xs:my-20 md:my-24 lg:my-28 lg:grid-cols-2 xl:my-32">
      <div class="grid grid-cols-1 justify-items-center md:grid-cols-2 lg:grid-cols-1">
        <div class="max-w-[400px] self-center pb-14 md:pb-0 lg:pb-16">
          <div class="mx-auto mb-16 -mt-4 block w-auto max-w-[280px] xs:hidden">
            <Logo class="force-light" variation="mobile" />
          </div>
          <h1 class="text-white">
            <span class="hidden xs:block">{{ t('content.home.header_prefix') }}</span>
            <span class="hidden xs:block">Add to Calendar Button</span>
            <span class="block text-4xl opacity-70 xs:hidden">{{ t('content.home.header_slogan') }}</span>
          </h1>
          <h2 class="mt-5 hidden italic text-white opacity-70 xs:block">{{ t('content.home.header_slogan') }}</h2>
        </div>
        <div class="grid max-w-[400px] grid-cols-1  justify-items-center gap-6 self-center xs:grid-cols-2 md:grid-cols-1 lg:grid-cols-2">
          <div class="hidden xs:block">
            <RouterLink class="button-secondary order-first w-40 text-white md:order-last lg:order-first" v-if="locale=='en'" :to="{ name: 'home', hash: '#installation', params: { locale } }">
              {{ t('labels.getStarted') }}
            </RouterLink>
            <RouterLink class="button-secondary order-first w-40 text-white md:order-last lg:order-first" v-else :to="{ name: 'home-i18n', hash: '#installation', params: { locale } }">
              {{ t('labels.getStarted') }}
            </RouterLink>
          </div>
          <div>
            <RouterLink class="button-primary w-40 text-white" v-if="locale=='en'" :to="{ name: 'home', hash: '#demo', params: { locale } }">
              {{ t('labels.demo') }}
              <ArrowDownIcon class="-mt-1 ml-2 inline-block h-4 w-4 xs:hidden" aria-hidden="true" />
            </RouterLink>
            <RouterLink class="button-primary w-40 text-white" v-else :to="{ name: 'home-i18n', hash: '#demo', params: { locale } }">
              {{ t('labels.demo') }}
              <ArrowDownIcon class="-mt-1 ml-2 inline-block h-4 w-4 xs:hidden" aria-hidden="true" />
            </RouterLink>
          </div>
        </div>
      </div>
      <div class="hidden self-center lg:block">
        <img :alt="t('labels.markThis')" class="hidden xl:block" width="400" height="198" :src="OvalLight" />
        <img alt="Add to Calendar Button" width="150" height="150" :src="HeroImage" class="animate-bounce-minimal xl:-mt-[180px] xl:ml-[125px]" />
      </div>
    </div>
  </div>
  <div class="hidden sm:block"><StatsBar /></div>
</template>
