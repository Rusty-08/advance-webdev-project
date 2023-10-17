<script setup>
    import { faMinus, faPlus } from '@fortawesome/free-solid-svg-icons'
    import { ref, onMounted, watch, watchEffect, computed } from 'vue'

    let containerProperties = [
        {   
            model: ref(''),
            name: 'Flex-direction',
            options: [
                'flex-column',
                'flex-column-reverse',
                'flex-row',
                'flex-row-reverse'
            ],
            isSelected: ref(false)
        },
        { 
            model: ref(''),
            name: 'Align-items',
            options: [
                'align-items-start',
                'align-items-end',
                'align-items-center',
                'align-items-baseline',
                'align-items-stretch',
            ],
            isSelected: ref(false)
        },
        { 
            model: ref(''),
            name: 'Justify-content',
            options: [
                'justify-content-start',
                'justify-content-end',
                'justify-content-center',
                'justify-content-between',
                'justify-content-around',
                'justify-content-evenly',
            ],
            isSelected: ref(false)
        },
        {   
            model: ref(''),
            name: 'Gap',
            options: [
                'gap-1',
                'gap-2',
                'gap-3',
                'gap-4',
                'gap-5',
            ],
            isSelected: ref(false)
        },
        { 
            model: ref(''),
            name: 'Flex-wrap',
            options: [
                'flex-wrap',
                'flex-nowrap',
                'flex-wrap-reverse'
            ],
            isSelected: ref(false)
        }
    ]

    let itemProperties = [
        {
            model: ref(''),
            name: 'Width',
            options: [
                'w-auto',
                'w-25',
                'w-50',
                'w-75',
                'w-100'
            ],
            isSelected: ref(false)
        },
        {
            model: ref(''),
            name: 'Height',
            options: [
                'h-auto',
                'h-25',
                'h-50',
                'h-75',
                'h-100'
            ],
            isSelected: ref(false)
        },
        { 
            model: ref(''),
            name: 'Align-self',
            options: [
                'align-self-start',
                'align-self-end',
                'align-self-center',
                'align-self-baseline',
                'align-self-stretch',
            ],
            isSelected: ref(false)
        },
        { 
            model: ref(''),
            name: 'Align-items',
            options: [
                'align-items-start',
                'align-items-end',
                'align-items-center',
                'align-items-baseline',
                'align-items-stretch',
            ],
            isSelected: ref(false)
        },
        { 
            model: ref(''),
            name: 'Justify-content',
            options: [
                'justify-content-start',
                'justify-content-end',
                'justify-content-center',
                'justify-content-between',
                'justify-content-around',
                'justify-content-evenly',
            ],
            isSelected: ref(false)
        },
        {
            model: ref(''),
            name: 'Flex-grow',
            options: [
                'flex-grow-0',
                'flex-grow-1',
            ],
            isSelected: ref(false)
        },
        {
            model: ref(''),
            name: 'Flex-shrink',
            options: [
                'flex-shrink-0',
                'flex-shrink-1',
            ],
            isSelected: ref(false)
        },
        {
            model: ref(''),
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
            ],
            isSelected: ref(false)
        },
    ]

    let itemArr = ref([])
    let itemNumber = ref(1)

    let containerClasses = computed(() => getClasses(containerProperties))
    let itemClasses = computed(() => getClasses(itemProperties))

    const getClasses = (elements) => {
        
        elements
            .filter(e => e.model.value == '')
            .map(e => e.isSelected.value = false)

        let isSelectedItems = elements.filter(e => e.model.value !== '')
        isSelectedItems.map(e => e.isSelected.value = true)
        
        return isSelectedItems.map(e => e.model.value).join(' ')
    }

    // update the value of each container model/properties when changed
    const updateContainerClasses = (index, value) => {
        containerProperties[index].model.value = value
    }

    // update the value of each item model/properties when changed
    const updateItemClasses = (index, value) => {
        itemProperties[index].model.value = value
    }

    const displayItems = () => {
        // remove all the items
        itemArr.value.splice(0, itemArr.value.length)
        
        // add new number of items
        for(let i = 0; i < itemNumber.value; i++) {
            itemArr.value.push(`Item ${i + 1}`)
        }
    }

    const minusItem = () => {
        if(itemNumber.value === 1) return
        itemNumber.value--
        displayItems()
    }

    const addItem = () => {
        if(itemNumber.value === 5) return
        itemNumber.value++
        displayItems()
    }

    // initiate initial number of items
    displayItems();

</script>

<template>
    <section class="tab-pane fade" id="interactive-visualizer" role="tabpanel" aria-labelledby="interactive-visualizer-link" tabindex="0">
        <div class="container-fluid vh-100 p-4 py-3 shadow-sm">
            <div class="page-content gap-3 card p-4 w-100 h-100 flex-row d-flex justify-content-center align-items-center">
                <div class="properties d-flex flex-column card p-4 h-100">
                    <h2 class="fs-6 fw-semibold border-bottom pb-2">Flexbox bootstrap properties</h2>
                    <div class="d-flex flex-column gap-2 mt-2 w-100 h-100">

                        <!-- CONTAINER PROPERTIES -->
                        <div class="container-properties w-100">
                            <h2 class="fs-8 d-flex justify-content-center align-items-center text-center rounded-1 mb-3">Container</h2>
                            <div class="d-flex gap-2 flex-wrap">
                                <select 
                                    v-for="(select, index) in containerProperties"
                                    class="form-select fs-8 mb-2" 
                                    :class="{ 'itemSelected shadow-sm': select.isSelected.value == true }"
                                    :key="index"
                                    v-model="select.model.value"
                                    @change="updateContainerClasses(index, $event.target.value)"
                                > 
                                    <option value="">{{ select.name }}</option>                               
                                    <option
                                        v-for="(item, index) in select.options"
                                        :value="item"
                                        :key="index"
                                    >
                                        {{ item }}
                                    </option>
                                </select>
                            </div>
                        </div>

                        <!-- ITEM PROPERTIES -->
                        <div class="item-properties w-100">
                            <div class="mb-3 gap-2 d-flex align-items-center justify-content-center">
                                <h2 class="fs-8 m-0 d-flex justify-content-center align-items-center flex-grow-1 text-center rounded-1">Item</h2>
                                <div class="item-number position-relative rounded-1 gap-3 d-flex align-items-center justify-content-center">
                                    <button 
                                        class="btn py-0 fs-8"
                                        @click="minusItem()"
                                    >
                                        <font-awesome-icon :icon="faMinus" />
                                    </button>
                                    <span class="fw-semibold position-absolute">{{ itemNumber }}</span>
                                    <button 
                                        class="btn py-0 fs-8"
                                        @click="addItem()"
                                    >
                                        <font-awesome-icon :icon="faPlus" />
                                    </button>
                                </div>
                            </div>
                            <div class="d-flex flex-wrap gap-2">
                                <select 
                                    v-for="(select, index) in itemProperties"
                                    class="form-select fs-8 mb-2"
                                    :class="{ 'itemSelected shadow-sm': select.isSelected.value == true }" 
                                    :key="index"
                                    v-model="select.model.value"
                                    @change="updateItemClasses(index, $event.target.value)"
                                >
                                    <option value="">{{ select.name }}</option>
                                    <option
                                        v-for="(item, index) in select.options"
                                        :value="item"
                                        :key="index"
                                    >
                                        {{ item }}
                                    </option>
                                </select>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="container-preview pt-2 container d-flex flex-column">
                    <div class="container-header mb-1 d-flex align-items-center justify-content-between">
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
                    <div class="tab-content h-100" id="pills-tabContent">
                        <div class="tab-pane h-100 fade show active" id="visual-tab" role="tabpanel" aria-labelledby="visual" tabindex="0">
                            <div 
                                class="container-board d-flex card"
                                :class="containerClasses"
                            >
                                <div 
                                    v-for="item in itemArr"
                                    class="card item-card d-flex fw-semibold fs-7 text-light"
                                    :class="itemClasses"
                                >
                                    {{ item }}
                                </div>
                            </div>
                        </div>
                        <div class="tab-pane h-100 fade" id="code-tab" role="tabpanel" aria-labelledby="code" tabindex="0">
                            <div class="container-board card">
                                Code
                            </div>
                        </div>
                    </div>
                </div>
            </div>  
        </div>
    </section> 
</template>

<style setup>
    .properties {
        flex: 1;
        border-width: 2px;
        border-radius: 0;
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
        width: 400px;
        height: 400px !important;
    }
    .container-board {
        height: 100%;
        border: 2px solid var(--secondary-text-color);
        border-radius: 0;
        transition: var(--transition-275s);
    }
    .container-properties h2,
    .item-properties h2 {
        border: 2px solid var(--secondary-text-color);
        color: var(--tertiary-text-color);
        height: 2rem;
    }
    .item-number {
        border: 2px solid var(--secondary-text-color);
        height: 2rem;
    }
    .item-number button {
        height: 100%;
        color: var(--secondary-text-color);
    }
    .item-number button:active {
        border-color: transparent !important;
    }
    .item-number button:active svg {
        transform: scale(1.1) !important;
    }
    .item-number-icon {
        right: 1rem
    }
    .container-properties .form-select,
    .item-properties .form-select {
        width: 49% !important;
        color: var(--tertiary-text-color);
    }
    .itemSelected {
        border-color: var(--tertiary-text-color) !important;
    }
    .item-card {
        border-radius: 0;
        border: 2px solid var(--tertiary-text-color);
        background-color: var(--secondary-text-color);
        transition: var(--transition-275s);
    }
</style>