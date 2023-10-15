<script setup>
    import Sidebar from './components/Sidebar.vue';
    
    // COLOR CONVERSION PAGE
    import HexToRgba from './components/HexToRgba.vue';
    import RgbaToHex from './components/RgbaToHex.vue';
    import VoiceToText from './components/VoiceToText.vue';
    import TextToVoice from './components/TextToVoice.vue';
    import InteractiveVisualizer from './components/InteractiveVisualizer.vue'
    import ToolFour from './components/ToolFour.vue'
    import ToolFive from './components/ToolFive.vue'
    import ToolSix from './components/ToolSix.vue'
</script>

<template>
    <Sidebar />
    <!-- PAGE CONTENT -->
    <main class="tab-content" id="main-body">
        <!-- TABPANE 1 -->
        <HexToRgba />
        <!-- TABPANE 2 -->
        <RgbaToHex />
         <!-- TABPANE 1 -->
         <TextToVoice />
        <!-- TABPANE 2 -->
        <VoiceToText />
        <!-- TABPANE 3 -->
        <InteractiveVisualizer />
        <!-- TABPANE 4 -->
        <ToolFour />
        <!-- TABPANE 5 -->
        <ToolFive />
        <!-- TABPANE 6 -->
        <ToolSix />
    </main>
</template>

<style setup>
    .tab-content section {
        width: calc(100% - var(--sidebar-width));
        min-height: 100vh !important;
        margin-left: var(--sidebar-width);
    }
</style>