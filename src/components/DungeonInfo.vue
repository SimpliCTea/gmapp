<template>
  <div id="DungeonInfo" v-if="activeTomb !== null">
    <h1>Dungeon Info</h1>
    <p>Size: {{ activeTomb.size }}</p>
    <p>Levels: {{ activeTomb.levels.length }}</p>
    <ul class="nav nav-tabs">
      <li
        class="nav-item"
        v-for="level of activeTomb.levels"
        :key="level.location"
      >
        <a class="nav-link" href="#" @click="setActiveLevel(level.location)"
          >Level: {{ level.location }}</a
        >
      </li>
    </ul>
    <level-section :rooms="getActiveLevelData().rooms"></level-section>
    <!-- <component :is="activeLevel"></component> -->
    <!-- <div v-for="level of levels" :key="level.location">
            <h2>Level #{{level.location}}</h2>
            <room-card v-for="room in level.rooms" :key="room.type + Math.random()" :title="room.type" :room_data="room"></room-card>
        </div> -->
  </div>
</template>

<script>
import LevelSection from "./LevelSection.vue";

export default {
  name: "DungeonInfo",
  components: {
    LevelSection
  },
  data() {
    return {
      activeLevel: 0,
      activeTomb: null,
      tombs: [
        {
          size: 12,
          activeLevel: 0,
          levels: [
            {
              location: 2,
              rooms: [
                {
                  type: "Trap Room",
                  content: [
                    "Mimic Trap",
                    "Poison Gas Trap",
                    "Thunder Trap",
                    "Dart Trap",
                    "Dart Trap",
                    "Nightmare Trap"
                  ]
                },
                {
                  type: "Unfinished Room"
                },
                {
                  type: "Grave Room",
                  content: [
                    "Corroded Mummy",
                    "Corroded Mummy",
                    "Corroded Mummy",
                    "Preserved Mummy",
                    "Preserved Mummy"
                  ]
                }
              ]
            },
            {
              location: 1,
              rooms: [
                {
                  type: "Grave Room",
                  content: ["Polymorph Trap", "Corroded Mummy"]
                },
                {
                  type: "Grave Room",
                  content: [
                    "Living Mummy",
                    "Corroded Mummy",
                    "Preserved Mummy",
                    "Nightmare Trap"
                  ]
                },
                {
                  type: "Story Room"
                },
                {
                  type: "Treasury Room",
                  content: {
                    coins: 0,
                    art: 1000,
                    gems: 600,
                    items: ["Test"],
                    chests: [
                      "Polymorph Trap",
                      {
                        coins: 200,
                        gems: 200,
                        art: 0,
                        items: []
                      },
                      {
                        coins: 700,
                        gems: 900,
                        art: 400,
                        items: ["Unnamed Item"]
                      }
                    ]
                  }
                }
              ]
            },
            {
              location: 0,
              rooms: [
                {
                  type: "Story Room"
                },
                {
                  type: "Story Room"
                }
              ]
            },
            {
              location: -1,
              rooms: [
                {
                  type: "Grave Room",
                  content: [
                    "Corroded Mummy",
                    "Corroded Mummy",
                    "Corroded Mummy",
                    "Living Mummy"
                  ]
                },
                {
                  type: "Grave Room",
                  content: [
                    "Corroded Mummy",
                    "Preserved Mummy",
                    "Thunder Trap",
                    "Mimic Trap"
                  ]
                },
                {
                  type: "Grave Room",
                  content: ["Corroded Mummy"]
                },
                {
                  type: "Grave Room",
                  content: ["Preserved Mummy"]
                },
                {
                  type: "Grave Room",
                  content: ["Corroded Mummy", "Living Mummy"]
                }
              ]
            }
          ]
        }
      ]
    };
  },
  methods: {
    setActiveLevel(lvl) {
      this.activeLevel = lvl;
    },
    getActiveLevelData() {
      return this.activeTomb.levels.filter(
        x => x.location == this.activeLevel
      )[0];
    },
    setActiveTomb() {
      this.activeTomb = this.tombs[0];
    }
  },
  created() {
    this.setActiveTomb();
  }
};
</script>

<style></style>
