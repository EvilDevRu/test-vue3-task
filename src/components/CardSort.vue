<template>
  <v-tooltip text="Сортировка по рейтингу">
    <template v-slot:activator="{ props }">
      <v-btn
          v-bind="props"
          icon="mdi-sort"
          density="compact"
          variant="tonal"
          class="sort-btn"
          color="blue"
          @click="sortList" />
    </template>
  </v-tooltip>
</template>

<script setup lang="ts">
import {inject, ref, watch} from "vue";

const SORT_ASC = 1;
const SORT_DESC = 2;
const SORT_NONE = null;

const firstList = inject('firstList');
const secondList = inject('secondList');
const lastList = inject('lastList');

const props = defineProps({
  options: {},
});

let cards = ref([]);
const sortDir = ref(SORT_NONE);

switch (props.options.id) {
  case 1:
    cards = firstList;
    break;
  case 2:
    cards = secondList;
    break;
  case 3:
    cards = lastList;
    break;
}

const sortList = () => {
  switch (sortDir.value) {
    case SORT_ASC:
      sortDir.value = SORT_DESC;
      break

    case SORT_DESC:
      sortDir.value = SORT_NONE;
      break;

    default:
      sortDir.value = SORT_ASC;
      break;
  }
};

watch(sortDir, (current) => {
  console.log('sort', sortDir.value);

  switch (current) {
    case SORT_ASC:
      cards.value = cards.value.sort((a, b) => a.rating.rate - b.rating.rate);
      break

    case SORT_DESC:
      cards.value = cards.value.sort((a, b) => b.rating.rate - a.rating.rate);
      break;

    default:
      cards.value = cards.value.sort((a, b) => a.id - b.id);
      break;
  }
});
</script>