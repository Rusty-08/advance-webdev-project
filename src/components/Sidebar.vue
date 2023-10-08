<script setup>
    import { faScrewdriverWrench } from '@fortawesome/free-solid-svg-icons'

    const sidebar = [
        { 
            category: 'COLOR CONVERSION',
            links: ['Hex to RGBA', 'RGBA to Hex'],
            icon: [faScrewdriverWrench, faScrewdriverWrench] 
        }, 
        { 
            category: 'FLEXBOX', 
            links: ['Tool Three', 'Tool Four'], 
            icon: [faScrewdriverWrench, faScrewdriverWrench] 
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
    };

</script>

<template>
    <aside class="side-bar vh-100">

        <!-- SIDEBAR HEADER -->
        <div class="logo-header d-flex align-items-center px-4">
            <img src="/img/tool-logo.png" alt="">
            <h5 class="fs-6 m-0">MICRO TOOLS</h5>
        </div>

        <!-- SIDEBAR LINKS -->
        <div class="nav flex-column nav-pills my-4 px-1"
            id="pills-tab"
            role="tablist"
            aria-orientation="vertical"
        >
            <ul v-for="(section, index) in sidebar" class="navbar-nav w-100 px-2" :key="index">
                <span class="nav-link-header">{{ section.category }}</span>
                <li v-for="(link, linkIndex) in section.links" :key="linkIndex" class="nav-item" role="presentation">
                    <a :href="`#${getLink(link)}`"
                        class="nav-link"
                        :class="{ 'active': link == getLinkLists()[0] }"
                        :id="`${getLink(link)}-link`"
                        data-bs-toggle="pill"
                        :data-bs-target="`#${getLink(link)}`"
                        role="tab"
                        type="button"
                        :aria-controls="getLink(link)"
                        :aria-selected="link == getLinkLists()[0] ? true : false"
                    >
                        <font-awesome-icon :icon="section.icon[linkIndex]" />
                        {{ link }}
                    </a>
                </li>
            </ul>
        </div>
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
</style>