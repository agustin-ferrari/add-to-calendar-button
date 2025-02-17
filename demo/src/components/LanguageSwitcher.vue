<script setup lang="ts">
import { watch, ref } from 'vue'
import { useRouter } from "vue-router";
import { useI18n } from 'vue-i18n';
import {
    Listbox,
    ListboxButton,
    ListboxOptions,
    ListboxOption,
  } from '@headlessui/vue';
import { LanguageIcon, CheckIcon, ChevronDownIcon } from '@heroicons/vue/20/solid';

const router = useRouter();
const { locale } = useI18n();

const languages = [
  {id: 'en', label: 'English'},
  {id: 'de', label: 'Deutsch'},
];

const currentId = languages.findIndex(e => e.id === locale.value);
const currentLanguage = ref(languages[currentId]);
// sync to switch locale from router locale path
watch(router.currentRoute, route => {
  if (route.name == 'home') {
    currentLanguage.value = languages[0];
  } else {
    const newId = languages.findIndex(e => e.id === locale.value);
    currentLanguage.value = languages[newId];
  }
});
// push route when locale change has been detected
watch(currentLanguage, val => {
  if ((router.currentRoute.value.name == 'home-i18n' || router.currentRoute.value.name == 'home') && val.id == 'en') {
    router.push({
      name: 'home',
      params: { locale: val.id }
    });
  } else if (router.currentRoute.value.name == 'home' && val.id != 'en') {
    router.push({
      name: 'home-i18n',
      params: { locale: val.id }
    });
  } else {
    router.push({
      name: router.currentRoute.value.name!,
      params: { locale: val.id }
    });
  }
});
</script>

<template>
  <div class="mx-auto w-36">
    <Listbox v-model="currentLanguage">
      <div class="relative">
        <ListboxButton class="language-switch-btn">
          <span class="icon">
            <LanguageIcon class="h-5 w-5" aria-hidden="true" />
          </span>
          <span class="label">{{ currentLanguage.label }}</span>
          <span class="arrow">
            <ChevronDownIcon class="h-5 w-5 text-gray-400 transition-transform ui-open:rotate-180" aria-hidden="true" />
          </span>
        </ListboxButton>

        <transition leave-active-class="transition duration-100 ease-in" leave-from-class="opacity-100" leave-to-class="opacity-0">
          <ListboxOptions class="language-switch-list">
            <ListboxOption v-slot="{ selected }" v-for="languageOption in languages" :key="languageOption.id" :value="languageOption" as="template">
              <li>
                <span class="label">{{ languageOption.label }}</span>
                <span v-if="selected" class="check">
                  <CheckIcon class="h-5 w-5" aria-hidden="true" />
                </span>
              </li>
            </ListboxOption>
          </ListboxOptions>
        </transition>
      </div>
    </Listbox>
  </div>
</template>

<style scoped>
.language-switch-btn {
  @apply relative w-full cursor-pointer rounded-lg bg-zinc-50 py-2 pl-9 pr-10 text-left shadow hover:bg-white hover:shadow-md focus:outline-none focus-visible:ring focus-visible:ring-secondary focus-visible:ring-opacity-75 dark:bg-zinc-700 dark:hover:bg-zinc-600 sm:text-sm;
}

.language-switch-btn .icon {
  @apply pointer-events-none absolute inset-y-0 left-0 flex items-center pl-2;
}

.language-switch-btn .label {
  @apply block truncate;
}

.language-switch-btn .arrow {
  @apply pointer-events-none absolute inset-y-0 right-0 flex items-center pr-2;
}

.language-switch-list {
  @apply absolute mt-1 max-h-36 w-full overflow-auto rounded-md bg-white py-1 text-base shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none dark:bg-zinc-700 sm:text-sm;
}

.language-switch-list li {
  @apply relative cursor-pointer select-none py-2 pl-10 pr-4 text-left ui-active:bg-secondary-light ui-active:text-zinc-900;
}

.language-switch-list li .label {
  @apply block truncate ui-selected:font-semibold;
}

.language-switch-list li .check {
  @apply absolute inset-y-0 left-0 flex items-center pl-3 text-secondary;
}
</style>
