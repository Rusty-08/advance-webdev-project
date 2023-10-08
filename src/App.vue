<script setup>
    import Sidebar from './components/Sidebar.vue'
    
    import ToolOne from './components/ToolOne.vue';
    import ToolTwo from './components/ToolTwo.vue';
    import ToolThree from './components/ToolThree.vue';
    import ToolFour from './components/ToolFour.vue';
    import ToolFive from './components/ToolFive.vue';
    import ToolSix from './components/ToolSix.vue';
</script>

<template>
    <Sidebar />
    <!-- PAGE CONTENT -->
    <main class="tab-content" id="main-body">
        <!-- TABPANE 1 -->
        <ToolOne />
        <!-- TABPANE 2 -->
        <ToolTwo />
        <!-- TABPANE 3 -->
        <ToolThree />
        <!-- TABPANE 4 -->
        <ToolFour />
        <!-- TABPANE 5 -->
        <ToolFive />
        <!-- TABPANE 6 -->
        <ToolSix />
    </main>
</template>