<template>
  <div class="row">
    <room-card
      v-for="room in rooms"
      :key="room.type + Math.random()"
      :title="room.type + ' (' + room.size + ')'"
      :room_content="room.content"
    ></room-card>
  </div>
</template>

<script>
import RoomCard from "./RoomCard.vue";

export default {
  props: ["rooms"],
  components: {
    RoomCard
  }
};
</script>

<style></style>
