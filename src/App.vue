<script setup>
import Sidebar from "./components/Sidebar.vue";
import HexToRgba from "./components/HexToRgba.vue";
import RgbaToHex from "./components/RgbaToHex.vue";
import VoiceToText from "./components/VoiceToText.vue";
import FlexboxVisualizer from "./components/FlexboxVisualizer.vue";
</script>

<template>
  <Sidebar />
  <!-- PAGE CONTENT -->
  <main class="tab-content" id="main-body">
    <!-- TABPANE 1 -->
    <HexToRgba />
    <!-- TABPANE 2 -->
    <RgbaToHex />
    <!-- TABPANE 3 -->
    <VoiceToText />
    <!-- TABPANE 4 -->
    <FlexboxVisualizer />
  </main>
</template>

<style setup>
.tab-content section {
  width: calc(100% - var(--sidebar-width));
  min-height: 100vh !important;
  margin-left: var(--sidebar-width);
  transition: var(--transition-275s);
}
</style>
