<template>
  <div class="songs">
    <div class="tabs">
      <Tabs
        type="small"
        :tabs="tabs"
        align="right"
        v-model="activeTabIndex"
        @tabChange="getSongs"
      />
    </div>
    <SongTable
      header-row-class-name="header-row"
      :songs="songs"
    />
  </div>
</template>

<script type="text/ecmascript-6">
import { getTopSongs } from '@/api/song'
import { createSong } from '@/utils/song'
import SongTable from '@/components/song-table'

export default {
  async created() {
    this.tabs = [
      { title: '全部', type: 0 },
      { title: '华语', type: 7 },
      { title: '欧美', type: 96 },
      { title: '日本', type: 8 },
      { title: '韩国', type: 16 },
    ]
    this.getSongs()
  },
  data() {
    return {
      activeTabIndex: 0,
      songs: []
    }
  },
  methods: {
    async getSongs() {
      const { data } = await getTopSongs(this.tabs[this.activeTabIndex].type)
      this.songs = data.map(song => {
        const { id, name, artists, duration, album: { picUrl, name: albumName } } = song
        return createSong({
          id,
          name,
          artists,
          duration,
          albumName,
          img: picUrl,
        })
      })
    }
  },
  components: {
    SongTable
  }
}
</script>

<style lang="scss">
.songs {
  padding: 12px;

  .header-row {
    display: none;
  }
}
</style>
