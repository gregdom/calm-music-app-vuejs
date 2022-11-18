<template>
  <div class="imgBg">
    <img :class="coverAnimation" :src="current.cover" />
  </div>
  <div class="container">
    <main>
      <div class="info">
        <h2 class="sound-title">
          {{ current.title }}
        </h2>
      </div>

      <div class="cover-wrapper">
        <img :class="coverAnimation" :src="current.cover" />
      </div>

      <div class="volumeBlock">
        <div class="block">
          <input
            type="range"
            class="volume-range"
            step="1"
            min="0"
            max="100"
            v-model="volume.range"
            @change="changeVolume"
          />
        </div>
      </div>

      <div class="controls desktop">
        <div>
          <button class="prev btnControls" @click="prev">
            <span class="material-symbols-outlined"> skip_previous </span>
          </button>
        </div>

        <div>
          <button class="play btnControls" v-if="!isPlaying" @click="play">
            <span class="material-symbols-outlined"> play_arrow </span>
          </button>
          <button class="pause btnControls" v-else @click="pause">
            <span class="material-symbols-outlined"> pause </span>
          </button>
        </div>

        <div>
          <button class="next btnControls" @click="next">
            <span class="material-symbols-outlined"> skip_next </span>
          </button>
        </div>
      </div>
    </main>

    <aside>
      <section class="playlist">
        <h3>Playlist</h3>
        <ul>
          <li
            v-for="sound in sounds"
            :key="sound.src"
            class="sound"
            @click="play(sound)"
          >
            <div class="cover-playlist">
              <img :src="sound.cover" />
            </div>
            <div class="details">
              <h2 class="sound-title">
                {{ sound.title }}
              </h2>
            </div>
          </li>
        </ul>
      </section>
    </aside>
  </div>

  <div class="controls mobo">
    <div>
      <button class="prev btnControls" @click="prev">
        <span class="material-symbols-outlined"> skip_previous </span>
      </button>
    </div>

    <div>
      <button class="play btnControls" v-if="!isPlaying" @click="play">
        <span class="material-symbols-outlined"> play_arrow </span>
      </button>
      <button class="pause btnControls" v-else @click="pause">
        <span class="material-symbols-outlined"> pause </span>
      </button>
    </div>

    <div>
      <button class="next btnControls" @click="next">
        <span class="material-symbols-outlined"> skip_next </span>
      </button>
    </div>
  </div>
</template>

<script>
import soundsArr from "./db/sounds";

export default {
  name: "App",
  data() {
    return {
      current: {},
      index: 0,
      coverAnimation: { cover: true, animated: false },
      isPlaying: false,
      sounds: soundsArr,
      player: new Audio(),
      volume: {
        active: false,
        range: 20,
      },
    };
  },

  methods: {
    setAnimation() {
      this.coverAnimation.animated = true;

      setTimeout(() => {
        this.coverAnimation.animated = false;
      }, 1000);
    },
    setCurrentSong() {
      this.current = this.sounds[this.index];
      this.play(this.current);
      this.setAnimation();
    },
    play(sound) {
      if (typeof sound.src !== "undefined") {
        this.current.isPlaying = false;
        this.current = sound;
        this.player.src = this.current.src;
      }

      this.player.play();
      this.isPlaying = true;

      this.setAnimation();
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.current.isPlaying = false;
      this.index = this.sounds.indexOf(this.current);
      this.index++;
      if (this.index > this.sounds.length - 1) {
        this.index = 0;
      }
      this.setCurrentSong();
    },
    prev() {
      this.current.isPlaying = false;
      this.index = this.sounds.indexOf(this.current);
      this.index--;
      if (this.index < 0) {
        this.index = this.sounds.length - 1;
      }
      this.setCurrentSong();
    },
    changeVolume() {
      this.player.volume = this.volume.range / 100;
    },
    changeVolumeDown() {
      if (this.volume.range == 0) {
        this.volume.range = 0;
      } else {
        this.volume.range = Number(this.volume.range) - 1;
        this.player.volume = Number(this.volume.range) / 100;
      }
    },
    changeVolumeUp() {
      if (this.volume.range == 100) {
        this.volume.range = 100;
      } else {
        this.volume.range = Number(this.volume.range) + 1;
        this.player.volume = Number(this.volume.range) / 100;
      }
    },
  },
  mounted() {
    this.current = this.sounds[this.index];
    this.player.src = this.current.src;
    this.player.volume = 0.2;
  },
};
</script>

<style>
@import "./assets/styles/main.css";
</style>
