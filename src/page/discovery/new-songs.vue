<template>
  <div
    class="new-songs"
    v-if="list.length"
  >
    <Title>最新音乐</Title>
    <div class="list-wrap">
      <div
        class="list"
        :key="listIndex"
        v-for="(list, listIndex) in thunkedList"
      >
        <SongCard
          class="song-card"
          v-for="(item,index) in list"
          v-bind="nomalizeSong(item)"
          :order="getSongOrder(listIndex, index)"
          :key="item.id"
          @click.native="onClickSong(item)"
        />
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions, mapMutations } from "vuex"
import { getNewSongs } from "@/api/discovery"
import Title from "@/base/title"
import SongCard from "@/components/song-card"
import { createSong } from "@/utils/song"

const songsLimit = 10
export default {
  async created() {
    const { result } = await getNewSongs()
    this.list = result
  },
  data() {
    return {
      chunkLimit: Math.ceil(songsLimit / 2),
      list: []
    }
  },
  methods: {
    getSongOrder(listIndex, index) {
      return listIndex * this.chunkLimit + index + 1
    },
    nomalizeSong(song) {
      const {
        id,
        name,
        song: {
          artists,
          album: { blurPicUrl },
          duration
        }
      } = song
      return createSong({ id, name, img: blurPicUrl, artists, duration })
    },
    onClickSong(song) {
      const nomalizedSong = this.nomalizeSong(song)
      this.startSong(nomalizedSong)
      this.setPlaylist({ data: this.normalizedSongs })
    },
    ...mapMutations(["setPlaylist"]),
    ...mapActions(["startSong"])
  },
  computed: {
    thunkedList() {
      return [
        this.list.slice(0, this.chunkLimit),
        this.list.slice(this.chunkLimit, this.list.length)
      ]
    },
    normalizedSongs() {
      return this.list.map(song => this.nomalizeSong(song))
    }
  },
  components: { Title, SongCard }
}
</script>

<style lang="scss" scoped>
.list-wrap {
  display: flex;

  .list {
    flex: 1;
  }

  .song-card {
    cursor: pointer;

    &:hover {
      background: var(--light-bgcolor);
    }
  }
}
</style>