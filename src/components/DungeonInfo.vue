<template>
    <div>
        <button type="button" class="btn btn-primary">Generate Dungeon</button>
    </div>
    <div id="DungeonInfo">
        <h1>Dungeon Info</h1>
        <p>
            Size: {{ size }}
        </p>
        <p>
            Levels: {{ levels.length }}
        </p>
        <div v-for="level of levels" :key="level.location">
            <h2>Level #{{level.location}}</h2>
            <p v-for="room in level.rooms" :key="room.type + Math.random()"> {{room.type}} </p>
        </div>
    </div>
</template>

<script>
export default {
    name: 'DungeonInfo',
    data () {
        return {
            size: 12,
            levels: [
                {
                    location: 2,
                    rooms: [
                        {
                            type: 'Trap Room',
                            traps: ['Mimic Trap', 'Poison Gas Trap', 'Thunder Trap', 'Dart Trap', 'Dart Trap', 'Nightmare Trap']
                        },
                        {
                            type: 'Unfinished Room'
                        },
                        {
                            type: 'Grave Room',
                            graves: ['Corroded Mummy','Corroded Mummy','Corroded Mummy','Preserved Mummy','Preserved Mummy']
                        }
                    ]
                },
                {
                    location: 1,
                    rooms: [
                        {
                            type: 'Grave Room',
                            graves: ['Polymorph Trap', 'Corroded Mummy']
                        },
                        {
                            type: 'Grave Room',
                            graves: ['Living Mummy', 'Corroded Mummy', 'Preserved Mummy', 'Nightmare Trap']
                        },
                        {
                            type: 'Story Room'
                        },
                        {
                            type: 'Treasury Room',
                            treasures: {
                                coins: 0,
                                art: 1000,
                                gems: 600,
                                items: [],
                                chests: [
                                    [
                                        'Polymorph Trap',
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
                                            items: ['Unnamed Item']
                                        }
                                    ]
                                ]
                            }
                        }
                    ]
                },
                {
                    location: 0,
                    rooms: [
                        {
                            type: 'Story Room'
                        },
                        {
                            type: 'Story Room'
                        }
                    ]
                },
                {
                    location: -1,
                    rooms: [
                        {
                            type: 'Grave Room',
                            graves: ['Corroded Mummy','Corroded Mummy','Corroded Mummy','Living Mummy']
                        },
                        {
                            type: 'Grave Room',
                            graves: ['Corroded Mummy','Preserved Mummy','Thunder Trap','Mimic Trap']
                        },
                        {
                            type: 'Grave Room',
                            graves: ['Corroded Mummy']
                        },
                        {
                            type: 'Grave Room',
                            graves: ['Preserved Mummy']
                        },
                        {
                            type: 'Grave Room',
                            graves: ['Corroded Mummy','Living Mummy']
                        }
                    ]
                }
            ]
        }
    }
}
</script>

<style>

</style>