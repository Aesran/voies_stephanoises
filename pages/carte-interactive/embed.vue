<template>
  <ClientOnly>
    <Map :features="features" class="h-full w-full" />
  </ClientOnly>
</template>

<script setup>
const { getRevName } = useConfig();

// https://github.com/nuxt/framework/issues/3587
definePageMeta({
  pageTransition: false,
  layout: 'embed'
});

const { data: voies } = await useAsyncData(() => {
  return queryContent('voies-cyclables').where({ _type: 'json' }).find();
});

const features = voies.value.map(voie => voie.features).flat();

const description = `Découvrez la carte interactive des ${getRevName()}. Itinéraires rue par rue. Plan régulièrement mis à jour pour une information complète.`;
const COVER_IMAGE_URL = 'https://raw.githubusercontent.com/Aesran/voies_stephanoises/refs/heads/main/assets/0-logo-carte.png';
useHead({
  title: `Carte à jour des ${getRevName()}`,
  meta: [
    // description
    { hid: 'description', name: 'description', content: description },
    { hid: 'og:description', property: 'og:description', content: description },
    { hid: 'twitter:description', name: 'twitter:description', content: description },
    // cover image
    { hid: 'og:image', property: 'og:image', content: COVER_IMAGE_URL },
    { hid: 'twitter:image', name: 'twitter:image', content: COVER_IMAGE_URL }
  ]
});
</script>
