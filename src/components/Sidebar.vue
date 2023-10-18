<script setup>
    import { faBox, faBrush, faChevronRight, faCubes, faEllipsis, faFileInvoice, faFillDrip, faMicrophoneLines, faMicroscope, faPalette, faRightToBracket, faScrewdriverWrench, faSquareFull, faTableCellsLarge } from '@fortawesome/free-solid-svg-icons'
    import { ref } from 'vue'

    let isSidebarShrinked = ref(false)

    const sidebar = [
        { 
            category: 'CONVERSION',
            links: ['Hex to RGBA', 'RGBA to Hex' , 'Voice to Text', 'Text to Voice'],
            icon: [faFillDrip, faBrush, faMicrophoneLines, faFileInvoice] 
        }, 
        { 
            category: 'FLEXBOX', 
            links: ['Interactive Visualizer', 'Tool Four'], 
            icon: [faCubes, faScrewdriverWrench] 
        },
        { 
            category: 'GENERATOR', 
            links: ['Tool Five', 'Tool Six'], 
            icon: [faScrewdriverWrench, faScrewdriverWrench] 
        }
    ]

    const getLinkLists = () => {
        let linkLists = []

        sidebar.forEach(link => {
            for(let i = 0; i < link.links.length; i++) {
                linkLists.push(link.links[i])
            }
        })

        return linkLists
    }

    const getLink = (link) => {
        return link.toLowerCase().split(' ').join('-')
    }

    const toggleSidebarWidth = () => {
        isSidebarShrinked.value = !isSidebarShrinked.value

        if(isSidebarShrinked.value) {
            document.documentElement.style.setProperty('--sidebar-width', '5.5rem');
        } else {
            document.documentElement.style.setProperty('--sidebar-width', '17rem');
        }
    };

</script>

<template>
    <aside 
        class="side-bar vh-100 position-relative"
        :class="{ 'ShrinkedSidebar': isSidebarShrinked }"
    >
        <!-- SIDEBAR HEADER -->
        <div class="logo-header d-flex align-items-center px-4">
            <img src="/img/tool-logo.png" alt="">
            <h5 
                class="fs-6 m-0"
                v-show="!isSidebarShrinked"
            >
                MICRO TOOLS
            </h5>
        </div>

        <!-- SIDEBAR LINKS -->
        <div class="nav flex-column nav-pills px-1"
            id="pills-tab"
            role="tablist"
            aria-orientation="vertical"
        >
            <ul class="navbar-nav w-100 px-2">
                <template v-for="section in sidebar">
                    <span 
                        class="nav-link-header"
                        v-if="!isSidebarShrinked"
                    >
                        {{ section.category }}
                    </span>
                    <font-awesome-icon v-else :icon="faEllipsis" class="nav-link-header ellipisis" />
                    
                    <li v-for="(link, linkIndex) in section.links" :key="linkIndex" class="nav-item" role="presentation">
                        <a  href=""
                            class="nav-link"
                            :class="{ 'active': link == getLinkLists()[0] }"
                            :id="`${getLink(link)}-link`"
                            data-bs-toggle="pill"
                            :data-bs-target="`#${getLink(link)}`"
                            role="tab"
                            :aria-controls="getLink(link)"
                            :aria-selected="link == getLinkLists()[0] ? true : false"
                        >
                            <font-awesome-icon :icon="section.icon[linkIndex]" />
                            {{ isSidebarShrinked ? '' : link }}
                        </a>
                    </li>
                </template>
            </ul>
        </div>
        <button 
            class="minimizerBtn btn"
            @click="toggleSidebarWidth()"
        >
            <font-awesome-icon 
                :icon="faChevronRight" 
                class="sidebar-minimizer position-absolute" 
            />     
        </button>
    </aside>
</template>

<style setup>
    .nav::-webkit-scrollbar {
        width: 4px;
    }
    .nav::-webkit-scrollbar-thumb {
        background-color: transparent;
        border-radius: 6px;
    }
    .nav:hover::-webkit-scrollbar-thumb {
        background-color: #282831;
    }
    .side-bar {
        position: fixed !important;
        z-index: 500;
    }
    .nav {
        height: calc(100% - --header-height);
    }
    .logo-header {
        border-bottom: var(--border-dashed);
        height: var(--header-height);
        gap: 0.7rem;
    }
    .logo-header img {
        width: 2rem;
        border-radius: 50%;
        border: 2px solid var(--primary-text-color);
        background-color: var(--secondary-text-color);
    }
    .logo-header h5 {
        font-weight: 600 !important;
        color: var(--primary-text-color);
        letter-spacing: 0.5px;
        white-space: nowrap !important;
    }
    .side-bar {
        background-color: var(--sidebar-background-color);
        border-right: var(--border-dashed);
        width: var(--sidebar-width);
        transition: var(--transition-275s);
    }
    .ShrinkedSidebar {
        width: 5.5rem !important;
    }
    .nav-link-header {
        color: var(--tertiary-text-color);
        font-size: 12.35px !important;
        font-weight: 600;
        padding: 1.5rem 0 0.7rem 0.8rem !important;
    }
    .ellipisis {
        font-size: 1.2rem !important;
        padding: 1.45rem 0 0.7rem 0rem !important;
    }
    .nav-item {
        padding: 0.15rem 0 !important;
        overflow: hidden;
    }
    .nav-link {
        color: var(--secondary-text-color) !important;
        white-space: nowrap;
        font-size: 14.3px !important;
        font-weight: 500 !important;
        padding: 8.45px 15px !important;
    }
    .nav-link svg {
        width: 2rem;
        font-size: 1rem;
    }
    .nav-link:hover {
        color: var(--primary-text-color) !important;
        background-color: rgba(19, 23, 35, 0.5);
    }
    .nav-link.active {
        color: var(--primary-text-color) !important;
        background-color: var(--active-link-background-color) !important;
    }
    .sidebar-minimizer {
        width: 1rem;
        height: 1rem;
        padding: 0.7rem;
        border-radius: 50%;
        background-color: var(--sidebar-background-color);
        right: -1rem;
        top: 1rem;
        color: var(--primary-text-color) !important;
    }
    .minimizerBtn {
        transition: var(--transition-175s);
    }
    .minimizerBtn:hover .sidebar-minimizer {
        background-color: var(--active-link-background-color);
    }
</style>