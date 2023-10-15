<script setup>
    import { ref, onMounted } from 'vue'

    const containerProperties = [
        { 
            name: 'Flex-direction',
            options: [
                'flex-column',
                'flex-column-reverse',
                'flex-row',
                'flex-row-reverse'
            ]
        },
        { 
            name: 'Align-items',
            options: [
                'align-items-start',
                'align-items-end',
                'align-items-center',
                'align-items-baseline',
                'align-items-stretch',
            ]
        },
        { 
            name: 'Justify-content',
            options: [
                'justify-content-start',
                'justify-content-end',
                'justify-content-between',
                'justify-content-around',
                'justify-content-evenly',
            ]
        },
        { 
            name: 'Flex-wrap',
            options: [
                'flex-wrap',
                'flex-nowrap',
                'flex-wrap-reverse'
            ]
        }
    ]

    const itemProperties = [
        {
            name: 'Width',
            options: [
                'w-auto',
                'w-25',
                'w-50',
                'w-75',
                'w-100'
            ]
        },
        {
            name: 'Height',
            options: [
                'h-auto',
                'h-25',
                'h-50',
                'h-75',
                'h-100'
            ]
        },
        { 
            name: 'Align-self',
            options: [
                'align-self-start',
                'align-self-end',
                'align-self-center',
                'align-self-baseline',
                'align-self-stretch',
            ]
        },
        { 
            name: 'Align-items',
            options: [
                'align-items-start',
                'align-items-end',
                'align-items-center',
                'align-items-baseline',
                'align-items-stretch',
            ]
        },
        { 
            name: 'Justify-content',
            options: [
                'justify-content-start',
                'justify-content-end',
                'justify-content-between',
                'justify-content-around',
                'justify-content-evenly',
            ]
        },
        {
            name: 'Flex-grow',
            options: [
                'flex-grow-0',
                'flex-grow-1',
            ]
        },
        {
            name: 'Flex-shrink',
            options: [
                'flex-shrink-0',
                'flex-shrink-1',
            ]
        },
        {
            name: 'Order',
            options: [
                'order-first',
                'order-last',
                'order-0',
                'order-1',
                'order-2',
                'order-3',
                'order-4',
                'order-5',
            ]
        },
    ];

</script>

<template>
    <section class="tab-pane fade" id="interactive-visualizer" role="tabpanel" aria-labelledby="interactive-visualizer-link" tabindex="0">
        <div class="container-fluid vh-100 p-4 py-3 shadow-sm">
            <div class="page-content gap-3 card p-4 w-100 h-100 flex-row d-flex justify-content-center align-items-end">
                <div class="properties d-flex flex-column card p-4 h-100">
                    <h2 class="fs-6 fw-semibold border-bottom pb-2">Flexbox bootstrap properties</h2>
                    <div class="d-flex flex-column gap-2 mt-2 w-100 h-100">

                        <!-- CONTAINER PROPERTIES -->
                        <div class="container-properties w-100">
                            <h2 class="fs-8 py-2 text-center rounded-1 mb-3">Container</h2>
                            <div class="d-flex gap-2 flex-wrap">
                                <select 
                                    class="form-select fs-8 mb-2" 
                                    aria-label="Default select example"
                                    v-for="(select, index) in containerProperties"
                                    :key="index"
                                >
                                    <option 
                                        selected
                                    >
                                        {{ select.name }}
                                    </option>
                                    
                                    <option
                                        v-for="item in select.options"
                                        :value="item"
                                    >
                                        {{ item }}
                                    </option>
                                </select>
                            </div>
                        </div>

                        <!-- ITEM PROPERTIES -->
                        <div class="item-properties w-100">
                            <h2 class="fs-8 py-2 text-center rounded-1 mb-3">Item</h2>
                            <div class="d-flex flex-wrap gap-2">
                                <select 
                                    class="form-select fs-8 mb-2" 
                                    aria-label="Default select example"
                                    v-for="(select, index) in itemProperties"
                                    :key="index"
                                >
                                    <option 
                                        selected
                                    >
                                        {{ select.name }}
                                    </option>
                                    
                                    <option
                                        v-for="item in select.options"
                                        :value="item"
                                    >
                                        {{ item }}
                                    </option>
                                </select>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="container-preview pt-2 container d-flex flex-column">
                    <div class="container-header mb-1 d-flex align-items-end justify-content-between">
                        <h2 class="m-0 fs-6">Container</h2>
                        <ul class="nav nav-pills" id="pills-tab" role="tablist">
                            <li class="nav-item" role="presentation">
                                <button class="nav-link rounded-start-pill active" id="visual" data-bs-toggle="pill" data-bs-target="#visual-tab" type="button" role="tab" aria-controls="visual-tab" aria-selected="true">Visual</button>
                            </li>
                            <li class="nav-item" role="presentation">
                                <button class="nav-link rounded-end-pill" id="code" data-bs-toggle="pill" data-bs-target="#code-tab" type="button" role="tab" aria-controls="code-tab" aria-selected="false">Code</button>
                            </li>
                        </ul>
                    </div>
                    <div class="container-board card tab-content" id="pills-tabContent">
                        <div class="tab-pane h-100 fade show active" id="visual-tab" role="tabpanel" aria-labelledby="visual" tabindex="0">
                            <div 
                                class="item-card d-flex fw-semibold fs-7 card text-light"
                            >
                                ITEM
                            </div>
                        </div>
                        <div class="tab-pane h-25 w-25 fade" id="code-tab" role="tabpanel" aria-labelledby="code" tabindex="0">
                            Code
                        </div>
                    </div>
                </div>
            </div>  
        </div>
    </section> 
</template>

<style setup>
    .properties {
        width: 500px;
        border-width: 2px;
        border-color: var(--secondary-text-color);
    }
    .container-header {
        height: 2rem !important;
    }
    .container-header .nav-link {
        font-size: 0.8rem !important;
        background-color: transparent;
        color: var(--sidebar-background-color) !important;
        border: 1px solid var(--sidebar-background-color) !important;
        font-weight: 400 !important;
    }
    #visual {
        padding: 0.3rem 1rem 0.3rem 1.2rem !important;
    }
    #code {
        padding: 0.3rem 1.2rem 0.3rem 1rem !important;
        border-left-color: transparent !important;
    }
    .container-header .nav-link.active {
        background-color: var(--sidebar-background-color) !important;
        color: var(--primary-text-color) !important;
    }
    .container-preview {
        flex: 1;
        height: 100% !important;
    }
    .container-board {
        height: 100%;
        border: 2px solid var(--secondary-text-color);
        border-radius: 0;
    }
    .container-properties h2,
    .item-properties h2 {
        background-color: var(--secondary-text-color);
        color: var(--primary-text-color);
    }
    .container-properties .form-select,
    .item-properties .form-select {
        width: 49% !important;
        color: var(--tertiary-text-color);
        border-color: var(--secondary-text-color);
    }
    .item-card {
        border-radius: 0;
        background-color: var(--sidebar-background-color);
    }
</style>