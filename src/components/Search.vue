<template>
    <section class="search">
        <section class="input">
            <input type="text" v-model="searchText" @keyup="search" placeholder="Saisir le nom d'une musique...">
        </section>
        <section class="tracks">
            <Track v-on:added="clearSearch()" v-for="track in tracks" :key="track.id" :track="track" />
        </section>
    </section>
</template>

<script>
import Track from './Track.vue';

export default {
    components: {
        Track
    },

    data() {
        return {
            searchText: '',
            tracks: []
        };
    },

    methods: {
        search() {
            let q = new URLSearchParams();
            q.append('q', this.searchText);
            fetch(`https://queue.nkir.ch/api/search/?${q.toString()}`).then(res => {
                res.json().then(data => {
                    this.tracks = data.tracks.items;
                });
            });
        },

        clearSearch() {
            this.searchText = '';
            this.tracks = [];
        },

        toggleQueue() {
            console.log('test');
            this.$router.push({ name: 'queue' });
        }
    }
}
</script>

<style scoped>
    input {
        width: 80%;
        height: 70px;
        border-radius: 10px;
        padding: 0 20px;
        font-size: 16px;
        border: none;
    }

    section.tracks:last-child {
        margin-bottom: 25%;
    }
</style>