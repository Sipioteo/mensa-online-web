<script setup>

import SideBar from "@/components/SideBar.vue";
import {GetEvents, GetUser, FileUrl} from "@/api.js";
import Topbar from "@/components/Topbar.vue";
import EventBlock from "@/components/EventBlock.vue";
import {ref} from "vue";
import SideBarTile from "@/components/SideBarTile.vue";


let listOfEventsOriginal = [];
const listOfEvents = ref([])
const searchModel = defineModel()

GetEvents().then((events) => {
  console.log(events);
  listOfEventsOriginal = events;
  listOfEvents.value = listOfEventsOriginal;
})

function searchEvent(event) {
  if (event.target.value === "") {
    listOfEvents.value = listOfEventsOriginal;
    return;
  }
  listOfEvents.value = listOfEventsOriginal.filter((e) => e.name.toLowerCase().includes(event.target.value.toLowerCase()));
}


</script>

<template>
  <main>
    <SideBar>
      <SideBarTile title="Home" icon="Home" route="/hub" ></SideBarTile>
      <SideBarTile title="Events" icon="Ticket" route="/events" selected></SideBarTile>
      <SideBarTile title="Documents" icon="Document" route="/documents"></SideBarTile>
      <SideBarTile title="Community" icon="People" route="/community"></SideBarTile>
      <SideBarTile title="Boutique" icon="Shop"  route="/boutique"></SideBarTile>
    </SideBar>
    <div class="main-data">
      <Topbar>
          <input type="text" class="searcher" placeholder="Cerca evento" @input="searchEvent($event)" v-model="searchModel">
      </Topbar>
      <div class="main-content">
        <div class="toolbar-buttons">
        </div>
        <EventBlock v-for="event in listOfEvents" :event="event"></EventBlock>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  display: grid;
  grid-template-columns: 300px minmax(0, 100%);
  min-height: 100vh;
  transition: all 0.5s;
}

.main-data {
  min-height: 100vh;
  height: 100%;
  width: 100%;
  background-color: var(--color-background-mute);
  display: flex;
  flex-direction: column;
}

.main-content {
  padding: 1rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  justify-content: center;
  align-items: center;
}

.toolbar-buttons {
  display: flex;
  justify-content: flex-end;
  width: 80%;
  gap: 1rem;
}

.searcher {
  width: 100%;
  flex: 1;
}

@media (max-width: 900px) {
  main {
    grid-template-columns: 100px minmax(0, 100%);
  }
}
</style>