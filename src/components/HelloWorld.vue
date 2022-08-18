<template>
    <div class="post">
        <div v-if="players" class="content overflow-x-auto">
            <table class="table w-full">
                <thead>
                    <tr>
                        <th>Character Name</th>
                        <th>World</th>
                    </tr>
                </thead>
                <tbody v-for="player in players" :key="player.playerName" @click.prevent="obtainItems(player)">
                    <label for="my-modal-3"></label><tr class="hover">
                        <td>{{player.playerName}}</td>
                        <td>{{ player.world }}</td>
                    </tr>
                </tbody>
            </table>
        </div>
        <label for="my-modal-3" class="btn modal-button">Open Modal</label>

        <input type="checkbox" id="my-modal-3" class="modal-toggle" />

        <div class="modal overflow-x-auto">
            <div class="modal-box relativeml overflow-x-auto">
                <div class="overflow-x-auto">
                    <label for="my-modal-3" class="btn btn-sm btn-circle absolute right-2 top-2"></label>
                    <table class="table table-compact w-full">
                        <thead>
                            <tr>
                                <th>Item Name</th>
                                <th>LootEventType</th>
                                <th>Date</th>
                            </tr>
                        </thead>
                        <tbody v-for="item in items" :key="item.timestamp">
                            <tr>
                                <td>{{item.itemName}}</td>
                                <td align="center">{{item.lootEventTypeName}}</td>
                                <td>{{new Date(item.timestamp)}}</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="js">
    import { defineComponent } from 'vue';

    export default defineComponent({
        data() {
            return {
                loading: false,
                post: null,
                players: null,
                items: null
            };
        },
        created() {
            // fetch the data when the view is created and the data is
            // already being observed
            this.fetchPlayerData();
        },
        watch: {
            // call again the method if the route changes
            '$route': 'fetchPlayerData',
        },
        methods: {
            fetchPlayerData() {
                this.players = null;
                this.loading = true;
                fetch('api/players')
                    .then(r => r.json())
                    .then(json => {
                        this.players = json;
                        this.loading = false;
                        return;
                    });
            },
            obtainItems(player) {
                this.loading = true;
                this.items = null;
                fetch(`api/item?player=${player.playerName}&world=${player.world}`)
                    .then(r => r.json())
                    .then(json => {
                        this.items = json;
                        this.loading = false;
                        document.getElementById('my-modal-3').checked = true;
                        return;
                    });

            },
        },
    });
</script>
<style scoped>
@import "@/css/main.css"
</style>