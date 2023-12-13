<script>
import axios from "axios";
export default {
  data: function () {
    return {
      songs: [],
      newSongParams: {},
      currentSong: {},
    };
  },
  created: function () {
    this.indexSongs();
  },
  methods: {
    indexSongs: function () {
      axios.get("/songs.json").then((response) => {
        console.log("songs index", response);
        this.songs = response.data;
      });
    },
    createSong: function () {
      axios
        .post("/songs.json", this.newSongParams)
        .then((response) => {
          console.log("songs create", response);
          this.songs.push(response.data);
          this.newSongParams = {};
        })
        .catch((error) => {
          console.log("songs create error", error.response);
        });
    },
    showSong: function (song) {
      this.currentSong = song;
      document.querySelector("#song-details").showModal();
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>New Song</h1>
    <div>
      Title:
      <input type="text" v-model="newSongParams.title" />
      Artist:
      <input type="text" v-model="newSongParams.artist" />
      Album:
      <input type="text" v-model="newSongParams.album" />
      Duration:
      <input type="text" v-model="newSongParams.duration" />
      <button v-on:click="createSong()">Create Song</button>
    </div>
    <h1>All Songs</h1>
    <div v-for="song in songs" v-bind:key="song.id">
      <h2>{{ song.title }}</h2>
      <p>Artist: {{ song.artist }}</p>
      <p>Albun: {{ song.album }}</p>
      <p>Duration: {{ song.duration }}</p>
      <button v-on:click="showSong(song)">More info</button>
    </div>
    <dialog id="song-details">
      <form method="dialog">
        <h1>Song Info</h1>
        <p>Title: {{ currentSong.title }}</p>
        <p>Artist: {{ currentSong.artist }}</p>
        <p>Album: {{ currentSong.album }}</p>
        <p>Duration: {{ currentSong.duration }}</p>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>
