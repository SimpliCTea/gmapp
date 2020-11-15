<template>
  <div>
    <button type="button" class="btn btn-primary" @click="createDungeon()">
      Generate Dungeon
    </button>
    <button type="button" class="btn btn-primary" @click="logWeightedRoll()">
      Roll Weighted Dice
    </button>
  </div>
  <dungeon-info></dungeon-info>
</template>

<script>
import DungeonInfo from "./components/DungeonInfo.vue";

export default {
  name: "App",
  data() {
    return {
      dungeons: []
    };
  },
  components: {
    DungeonInfo
  },
  methods: {
    rollDice(die) {
      return 1 + Math.floor(Math.random() * die);
    },
    d2() {
      return this.rollDice(2);
    },
    d4() {
      return this.rollDice(4);
    },
    d6() {
      return this.rollDice(6);
    },
    d8() {
      return this.rollDice(8);
    },
    d10() {
      return this.rollDice(10);
    },
    d12() {
      return this.rollDice(12);
    },
    d20() {
      return this.rollDice(20);
    },
    d100() {
      return this.rollDice(100);
    },
    rollWeightedDice(probList) {
      if (probList.reduce((a, b) => a + b * 100, 0) !== 100) {
        throw 'rollWeightedDice => Probabilites in parameter "probList" should sum up to 1 (=100%)';
      }
      const rollDie = function(probList, roll, currentProb = 0, outcome = 0) {
        if (roll <= currentProb) return outcome;
        return rollDie(
          probList.slice(1),
          roll,
          currentProb + probList[0],
          outcome + 1
        );
      };
      return rollDie(probList, Math.random());
    },
    logWeightedRoll() {
      const outcome = this.rollWeightedDice([0.35, 0.35, 0.2, 0.1]);
      console.log(outcome);
    },
    getDungeonSize() {
      return this.d12();
    },
    getDungeonLevels() {
      const numLevels = this.rollWeightedDice([0.3, 0.3, 0.25, 0.15]);
      const createLevels = function(numLevels, levels = []) {
        if (numLevels === 0) return levels.sort((a, b) => a - b);
        if (levels.length !== 0) {
          const highestLevel = Math.max.apply(Math, levels);
          const lowestLevel = Math.min.apply(Math, levels);
          const nextLevel =
            Math.random() < 0.75 ? lowestLevel - 1 : highestLevel + 1;
          levels.push(nextLevel);
        } else {
          levels.push(0);
        }
        return createLevels(numLevels - 1, levels);
      };
      return createLevels(numLevels);
    },
    distributeRoomSpace(
      levelNumbers,
      maxSpace,
      maxRoomSize,
      levelDict = { numRooms: [], rooms: [] }
    ) {
      if (levelNumbers.length === 0) return levelDict;
      const createRooms = function(
        self,
        remainingSpace,
        maxRoomSize,
        rooms = []
      ) {
        if (remainingSpace === 0) return rooms;
        const newRoom = self.rollDice(
          remainingSpace < maxRoomSize ? remainingSpace : maxRoomSize
        );
        rooms.push(newRoom);
        return createRooms(self, remainingSpace - newRoom, maxRoomSize, rooms);
      };
      const rooms = createRooms(this, maxSpace, maxRoomSize);
      levelDict.numRooms.push(rooms.length);
      levelDict.rooms.push(rooms);
      return this.distributeRoomSpace(
        levelNumbers.slice(1),
        maxSpace,
        maxRoomSize,
        levelDict
      );
    },
    getRoomType() {
      const roomTypes = {
        1: "Trap Room",
        2: "Grave Room",
        3: "Treasure Room",
        4: "Story Room or Corridor",
        5: "Unfinished Room"
      };
      const roomWeights = [0.15, 0.25, 0.15, 0.35, 0.1];
      const roomID = this.rollWeightedDice(roomWeights);
      return roomTypes[roomID];
    },
    getTrapType() {
      const trapTypes = {
        1: {
          type: "Mimic",
          detectionDC: 20
        },
        2: {
          type: "Poison Gas",
          detectionDC: 15,
          saveType: "Constitution",
          saveDC: 15,
          effect: "4d4 poison damage; 20ft radius; 1 min",
          saveEffect: "no effect"
        },
        3: {
          type: "Fire Burst",
          detectionDC: 15,
          saveType: "Dexterity",
          saveDC: 20,
          effect: "3d6 fire damage",
          saveEffect: ""
        },
        4: {
          type: "Ice Burst",
          detectionDC: 20,
          saveType: "Dexterity",
          saveDC: 15,
          effect: "2d8 ice damage; half movement until end of next turn",
          saveEffect: "half damage"
        },
        5: {
          type: "Darts",
          saveType: "Dexterity",
          saveDC: 15,
          effect: "4d4 piercing damage",
          saveEffect: "half damage"
        },
        6: {
          type: "Nightmare",
          detectionDC: 25,
          saveType: "Intelligence",
          saveDC: 15,
          effect: "2d8 psychic damage; restrained 1 minute",
          saveEffect: "half damage"
        },
        7: {
          type: "Sleep Gas",
          detectionDC: 20,
          saveType: "Constitution",
          saveDC: 20,
          effect: "sleep for 2d6h",
          saveEffect: "no effect"
        },
        8: {
          type: "Polymorph",
          detectionDC: 25,
          saveType: "Charisma",
          saveDC: 15,
          effect: "polymorphed for 1d4 days",
          saveEffect: "no effect"
        },
        9: {
          type: "Hold Creature",
          detectionDC: 25,
          saveType: "Wisdom",
          saveDC: 15,
          effect: "incapacitated for 1d4h",
          saveEffect: "no effect"
        },
        10: {
          type: "Radiant Burst",
          detectionDC: 20,
          saveType: "CON",
          saveDC: 15,
          effect: "1d12 radiant damage; blind for 2d4h",
          saveEffect: "no blindness"
        },
        11: {
          type: "Thunder",
          detectionDC: 15,
          saveType: "Constitution",
          saveDC: 25,
          effect: "2d8 thunder damage; deaf 1d4h",
          saveEffect: "half damage"
        },
        12: {
          type: "Necrotic Burst",
          detectionDC: 20,
          saveType: "Constitution",
          saveDC: 15,
          effect:
            "7d4 necrotic damage; if killed by this damage turn into zombie",
          saveEffect: "half damage"
        }
      };
      const trapID = this.d12();
      return trapTypes[trapID];
    },
    getTombType() {
      const tombTypes = {
        1: "Dead Mummy (corroded)",
        2: "Dead Mummy (preserved)",
        3: "Living Mummy",
        4: "Trap"
      };
      const tombWeights = [0.3, 0.3, 0.2, 0.2];
      const tombID = this.rollWeightedDice(tombWeights);
      return tombTypes[tombID];
    },
    getChestType() {
      const chestTypes = {
        1: "Trap",
        2: "Minor Chest",
        3: "Major Chest"
      };
      const chestWeights = [0.2, 0.5, 0.3];
      const chestID = this.rollWeightedDice(chestWeights);
      return chestTypes[chestID];
    },
    getTreasuryType() {
      const treasuryTypes = {
        1: "Insignificant",
        2: "Normal",
        3: "Rich",
        4: "Oppulent"
      };
      const treasuryWeights = [0.25, 0.4, 0.25, 0.1];
      const treasuryID = this.rollWeightedDice(treasuryWeights);
      return treasuryTypes[treasuryID];
    },
    determineRoomTypes(roomsPerLevel, roomsTypeList = []) {
      if (roomsPerLevel.length === 0) return roomsTypeList;
      const determineTypes = function(self, numRooms, roomTypes = []) {
        if (numRooms === 0) return roomTypes;
        const room = self.getRoomType();
        roomTypes.push(room);
        return determineTypes(self, numRooms - 1, roomTypes);
      };
      roomsTypeList.push(determineTypes(this, roomsPerLevel[0]));
      return this.determineRoomTypes(roomsPerLevel.slice(1), roomsTypeList);
    },
    computeTreasuryValue(treasuryType) {
      switch (treasuryType) {
        case 'Insignificant': {
          return {
            'art': 800 + this.d4() * 100,
            'gems': this.d10() * 10
          };
        }
        case 'Normal': {
          return {
            'art': 2500 + 2 * this.d4() * 100,
            'gems': 800 + this.d4() * 100,
            'items': this.d100() <= 10 ? 'Random permanent magic item' : 'No items'
          };
        }
        case 'Rich': {
          return {
            'art': 4000 + 4 * this.d4() * 100,
            'gems': 1500 + 3 * this.d4() * 100,
            'items': this.d100() <= 25 ? 'Random permanent magic item' : 'No items'
          };
        }
        case 'Oppulent': {
          return {
            'art': 10000 + 8 * this.d4() * 100,
            'gems': 4000 + 4 * this.d4() * 100,
            'items': this.d100() <= 50 ? '2 random permanent magic items' : 'Random permanent magic item'
          };
        }
      }
    },
    computeCoins(numCoins, coinTypes = ['pp','gp','ep','sp','cp'], coins = '') {
      if (numCoins === 0 || coinTypes.length === 0) return coins.trim();
      const amount = coinTypes.length === 1 ? numCoins : this.rollDice(numCoins);
      const coinType = coinTypes.splice(this.rollDice(coinTypes.length)-1, 1);
      return this.computeCoins(numCoins - amount, coinTypes, coins + amount + coinType + ' ')
    },
    fillChest(chestType) {
      switch (chestType) {
        case 'Minor Chest': {
          return {
            'coins': this.computeCoins((2 * this.d4() - 2) * 100),
            'gems': (2 * this.d4() - 2) * 100,
            'consumables': this.d100() <= 20 ? 'Random consumable magical item' : 'No consumable item',
            'items': this.d100() <= 5 ? 'Random permanent magical item' : 'No permanent item'
          };
        }
        case 'Major Chest': {
          return {
            'coins': this.computeCoins((4 * this.d4() - 4) * 100),
            'gems': (4 * this.d4() - 4) * 100,
            'consumables': this.d100() <= 45 ? 'Random consumable magical item' : 'No consumable item',
            'items': this.d100() <= 20 ? 'Random permanent magical item' : 'No permanent item'
          };
        }
      }
    },
    populateRoom(roomSize, roomType) {
      console.log('IN POPULATE LEVELS');
      console.info('TYPE: ' + roomType + ' || SIZE: ' + roomSize);
      const numObjects = roomSize * this.d4() - (roomSize - 1);
      const getObjects = function(getNewObject, numObjects, objectList = []) {
        if (numObjects === 0) return objectList;
        objectList.push(getNewObject());
        return getObjects(getNewObject, numObjects - 1, objectList);
      };
      switch (roomType) {
        case 'Trap Room': {
          console.log('HEEEEELP I AM IN A TRAAAAAAP!!!!!')
          return getObjects(this.getTrapType, numObjects);
        }
        case 'Grave Room': {
          const tombs = getObjects(this.getTombType, numObjects);
          return tombs.map(type =>
            type === 'Trap' ? this.getTrapType() : type
          );
        }
        case 'Treasure Room': {
          const treasuryType = this.getTreasuryType();
          const treasuryContents = this.computeTreasuryValue(treasuryType);
          const chests = getObjects(this.getChestType, numObjects);
          treasuryContents['chests'] = chests.map(type => type === 'Trap' ? this.getTrapType() : this.fillChest(type))
          return treasuryContents;
        }
        case 'Story Room or Corridor': {
          return [
            'This space was used for story-telling. The walls and statues depict everything that was important to the builders in life.'
          ];
        }
        case 'Unfinished Room': {
          return [
            'This room was never finished. There may be some tools lying around and some of the walls are blank.'
          ];
        }
        default: {
          return ['I HAVE NO PROPER ROOM TYPE!']
        }
      }
    },
    populateLevels(levels, populatedLevels = []){
      if (levels.length === 0) return populatedLevels;
      const getRooms = function(self, roomTypes, roomSizes, rooms = []) {
        if (roomTypes.length === 0) return rooms;
        rooms.push(self.populateRoom(roomTypes[0], roomSizes[0]));
        return getRooms(self, roomTypes.slice(1), roomSizes.slice(1), rooms)
      };
      populatedLevels.push(getRooms(this, levels[0][0], levels[0][1]));
      return this.populateLevels(levels.slice(1),populatedLevels);
    },
    zip(list, other) {
      return list.map((k,i) => [k, other[i]]);
    },
    createDungeon() {
      // const dungeon = {};
      const spacesPerLevel = this.getDungeonSize();
      const levels = this.getDungeonLevels();
      const roomDistribution = this.distributeRoomSpace(
        levels,
        spacesPerLevel,
        4
      );
      const roomTypes = this.determineRoomTypes(roomDistribution.numRooms);
      const roomSizesAndTypes = this.zip(roomDistribution.rooms, roomTypes);
      const populatedLevels = this.populateLevels(roomSizesAndTypes);
      console.log('Dungeon Size: ' + spacesPerLevel);
      console.log('Dungeon Levels: ' + levels);
      console.log('Dungeon roomsPerLevel:');
      console.info(roomDistribution);
      console.log('Dungeon roomTypes:');
      console.info(roomTypes);
      console.info(roomSizesAndTypes);
      console.log('Dungeon populated Levels:');
      console.info(populatedLevels);
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
