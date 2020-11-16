<template>
  <base-card :title="title">
    <div v-if="title.includes('Treasure Room')">
      <p>Art: {{ room_content.loot.art }}</p>
      <p>Gems: {{ room_content.loot.gems }}</p>
      <p>Coins: {{ room_content.loot.coins }}</p>
      <p>Items: {{ room_content.loot.items }}</p>
      <p v-for="(chest, num) in room_content.chests" :key="chest">
        Chest {{ num + 1 }}: {{ chest }}
      </p>
    </div>
    <div v-else>
      <p v-for="element in room_content" :key="element + Math.random()">
        {{ element }}
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
