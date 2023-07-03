<script setup>
  import { ref, onMounted } from 'vue'
  import { RouterLink, RouterView } from 'vue-router'
  import MenuItem from './components/MenuItem.vue';
  import MusicPlayer from './components/MusicPlayer.vue'
  import ChevronUp from 'vue-material-design-icons/ChevronUp.vue';
  import ChevronDown from 'vue-material-design-icons/ChevronDown.vue';
  import ChevronRight from 'vue-material-design-icons/ChevronRight.vue';
  import ChevronLeft from 'vue-material-design-icons/ChevronLeft.vue';

  import { useSongStore } from './stores/song'
  import { storeToRefs } from 'pinia';
  const useSong = useSongStore()
  const { isPlaying, currentTrack } = storeToRefs(useSong)

  onMounted(() => { isPlaying.value = false })

  let openMenu = ref(false)
</script>

<template>
    <div>
        <div 
          id="TopNav"
          class="
            w-[calc(100%-240px)] 
            h-[60px] 
            fixed 
            right-0 
            z-20 
            bg-[#101010] 
            bg-opacity-80 
            flex 
            items-center 
            justify-between
          "
        >
            <div class="flex items-center ml-6">
                <button type="button" class="rounded-full bg-black p-[1px] cursor-pointer">
                    <ChevronLeft fillColor="#FFFFFF" :size="30" />
                </button>
                <button type="button" class="rounded-full bg-black p-[1px] hover:bg-[#] ml-4 cursor-pointer">
                    <ChevronRight fillColor="#FFFFFF" :size="30" />
                </button>
            </div>

            <button @click="openMenu = !openMenu" :class="openMenu ? 'bg-[#282828]' : 'bg-black'"
                class="bg-black hover:bg-[#282828] rounded-full p-0.5 mr-8 mt-0.5 cursor-pointer">
                <div class="flex items-center">
                    <img 
                      class="rounded-full" 
                      width="27"
                      src="data:image/png;base64,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"
                    >
                    <div class="text-white text-[14px] ml-1.5 font-semibold">Cahya Maul Dev</div>
                    <ChevronDown v-if="!openMenu" @click="openMenu = true" fillColor="#FFFFFF" :size="25" />
                    <ChevronUp v-else @click="openMenu = false" fillColor="#FFFFFF" :size="25" />
                </div>
            </button>

            <span v-if="openMenu"
                class="fixed w-[190px] bg-[#282828] shadow-2xl z-50 rounded-sm top-[52px] right-[35px] p-1 cursor-pointer">
                <ul class="text-gray-200 font-semibold text-[14px]">
                    <li class="px-3 py-2.5 hover:bg-[#3E3D3D] border-b border-b-gray-600">Profile</li>
                    <li class="px-3 py-2.5 hover:bg-[#3E3D3D]">Log out</li>
                </ul>
            </span>
        </div>


        <div id="SideNav" class="h-[100%] p-6 w-[240px] fixed z-50 bg-black">
            <RouterLink to="/">
              <img width="125" src="/images/icons/spotify-logo.png">
            </RouterLink>
            <div class="my-8"></div>
            <ul>
                <RouterLink to="/">
                    <MenuItem class="ml-[1px]" :iconSize="23" name="Home" iconString="home" pageUrl="/" />
                </RouterLink>
                <RouterLink to="/search">
                    <MenuItem class="ml-[1px]" :iconSize="24" name="Search" iconString="search" pageUrl="/search" />
                </RouterLink>
                <RouterLink to="/library">
                    <MenuItem class="ml-[2px]" :iconSize="23" name="Your Library" iconString="library" pageUrl="/library" />
                </RouterLink>
                <div class="py-3.5"></div>
                <MenuItem :iconSize="24" name="Create Playlist" iconString="playlist" pageUrl="/playlist" />
                <MenuItem class="-ml-[1px]" :iconSize="27" name="Liked Songs" iconString="liked" pageUrl="/liked" />
            </ul>
            <div class="border-b border-b-gray-700"></div>
            <ul>
                <li class="font-semibold text-[13px] mt-3 text-gray-300 hover:text-white">My Playlist #1</li>
                <li class="font-semibold text-[13px] mt-3 text-gray-300 hover:text-white">My Playlist #2</li>
                <li class="font-semibold text-[13px] mt-3 text-gray-300 hover:text-white">My Playlist #3</li>
                <li class="font-semibold text-[13px] mt-3 text-gray-300 hover:text-white">My Playlist #4</li>
            </ul>
        </div>
    </div>

    <div
        class="
            fixed
            right-0
            top-0
            w-[calc(100%-240px)]
            overflow-auto
            h-full
            bg-gradient-to-b
            from-[#1C1C1C]
            to-black
        "
    >
        <div class="mt-[70px]"></div>
        <RouterView />
        <div class="mb-[100px]"></div>
    </div>

    <MusicPlayer v-if="currentTrack"/>
</template>



