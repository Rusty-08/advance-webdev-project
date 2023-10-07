<script setup>
    import { faHouse, faMessage, faUserPlus, faUserPen, faClipboardList, faGraduationCap, faScrewdriverWrench } from '@fortawesome/free-solid-svg-icons'

    const sidebar = [
        { 
            category: 'CATEGORY 1',
            links: ['Tool 1', 'Tool 2'],
            icon: [faScrewdriverWrench, faScrewdriverWrench] 
        }, 
        { 
            category: 'CATEGORY 2', 
            links: ['Tool 3', 'Tool 4'], 
            icon: [faScrewdriverWrench, faScrewdriverWrench] 
        },
        { 
            category: 'CATEGORY 3', 
            links: ['Tool 5', 'Tool 6'], 
            icon: [faScrewdriverWrench, faScrewdriverWrench] 
        }
    ]

    const linkLists = [
        'Tool 1',
        'Tool 2',
        'Tool 3',
        'Tool 4',
        'Tool 5',
        'Tool 6'
    ]
   
    const getLinkURL = (link) => {
        return '#' + link.toLowerCase().split(' ').join('-')
    }

    const getLink = (link) => {
        return link.toLowerCase().split(' ').join('-')
    };

</script>

<template>
    <aside class="side-bar vh-100 position-relative">
        <!-- SIDEBAR HEADER -->
        <h6 class="logo-header d-flex align-items-center px-4 text-light fs-6 fw-bold">
            <img src="/img/tool-logo.png" alt="">
            MACRO TOOLS
        </h6>

        <!-- SIDEBAR LINKS -->
        <div class="nav flex-column nav-pills my-4 px-1"
            id="pills-tab"
            role="tablist"
            aria-orientation="vertical"
        >
            <ul v-for="(section, index) in sidebar" class="navbar-nav w-100 px-2" :key="index">
                <span class="nav-link-header">{{ section.category }}</span>
                <li v-for="(link, linkIndex) in section.links" :key="linkIndex" class="nav-item" role="presentation">
                    <a :href="getLinkURL(link)"
                        class="nav-link"
                        :class="{ 'active': link == 'Tool 1' }"
                        :id="`${getLink(link)}-link`"
                        data-bs-toggle="pill"
                        :data-bs-target="getLinkURL(link)"
                        role="tab"
                        type="button"
                        :aria-controls="getLink(link)"
                        :aria-selected="link == 'Home' ? true : false"
                    >
                        <font-awesome-icon :icon="section.icon[linkIndex]" />
                        {{ link }}
                    </a>
                </li>
            </ul>
        </div>
    </aside>

    <!-- PAGE CONTENT -->
    <main class="tab-content" id="main-body">
        <section
            v-for="(link, linkIndex) in linkLists"
            class="tab-pane fade"
            :class="{ 'show active': linkIndex == 0 }"
            :id="getLink(link)"
            role="tabpanel"
            :aria-labelledby="`${getLink(link)}-link`"
            tabindex="0"
        >
            <div class="container-fluid vh-100 d-flex justify-content-center align-items-center">
                <h1 class="fw-medium fs-5 m-0">{{ `${link} Page` }} </h1>   
            </div>
        </section>
        
    </main>

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
    .side-bar {
        background-color: var(--sidebar-background-color);
        border-right: var(--border-dashed);
        width: var(--sidebar-width);
    }
    .nav-link-header {
        color: var(--tertiary-text-color);
        font-size: 12.35px !important;
        font-weight: 600;
        padding: 1.5rem 0 0.7rem 0.8rem !important;
    }
    .navbar-nav:first-child .nav-link-header {
        padding-top: 0 !important;
    }
    .nav-item {
        padding: 0.15rem 0 !important;
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
    .tab-content section {
        width: calc(100% - var(--sidebar-width));
        min-height: 100vh !important;
        margin-left: var(--sidebar-width);
    }
    .tab-content section h1 {
        color: var(--secondary-text-color);
    }
</style>