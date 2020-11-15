<template>
  <base-card :title="title">
    <div v-if="title !== 'Treasury Room'">
      <p v-for="element in room_content" :key="element + Math.random()">
        {{ element }}
      </p>
    </div>
    <div v-else>
      <p>Art: {{ room_content.art }}</p>
      <p>Gems: {{ room_content.gems }}</p>
      <p>Coins: {{ room_content.coins }}</p>
      <p v-for="item in room_content.items" :key="item">Items: {{ item }}</p>
      <p v-for="(chest, num) in room_content.chests" :key="chest">
        Chest {{ num + 1 }}: {{ chest }}
      </p>
    </div>
  </base-card>
</template>

<script>
import BaseCard from "./BaseCard.vue";

export default {
  props: {
    title: String,
    room_content: Object
  },
  components: {
    BaseCard
  }
};
</script>

<style></style>
