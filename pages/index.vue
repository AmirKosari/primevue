<template>
    <div :class="landingClass">
        <AppNews />
        <AppTopBar :showConfigurator="false" :showMenuButton="false" @darkswitch-click="onDarkModeToggle" />
        <HeroSection />
        <FeaturesSection />
        <UsersSection />
        <ThemeSection :theme="tableTheme" @table-theme-change="onTableThemeChange" />
        <BlockSection />
        <TemplateSection />
        <FooterSection />
    </div>
</template>

<script>
import BlockSection from '@/components/landing/BlockSection.vue';
import FeaturesSection from '@/components/landing/FeaturesSection.vue';
import FooterSection from '@/components/landing/FooterSection.vue';
import HeroSection from '@/components/landing/HeroSection.vue';
import TemplateSection from '@/components/landing/TemplateSection.vue';
import ThemeSection from '@/components/landing/ThemeSection.vue';
import UsersSection from '@/components/landing/UsersSection.vue';
import EventBus from '@/layouts/AppEventBus';
import AppNews from '@/layouts/AppNews';
import AppTopBar from '@/layouts/AppTopBar.vue';

export default {
    setup() {
        definePageMeta({
            layout: 'custom'
        });
    },
    props: {
        theme: {
            type: String,
            default: null
        }
    },
    data() {
        return {
            tableTheme: 'lara-light-green'
        };
    },
    themeChangeListener: null,
    mounted() {
        let afId = this.$route.query['af_id'];

        if (afId) {
            let today = new Date();
            let expire = new Date();

            expire.setTime(today.getTime() + 3600000 * 24 * 7);
            document.cookie = 'primeaffiliateid=' + afId + ';expires=' + expire.toUTCString() + ';path=/; domain:primefaces.org';
        }

        const preferredColorScheme = localStorage.getItem(this.$appState.colorSchemeKey);
        const prefersDarkColorScheme = window.matchMedia('(prefers-color-scheme: dark)').matches;

        if ((preferredColorScheme === null && prefersDarkColorScheme) || preferredColorScheme === 'dark') {
            this.replaceTableTheme('lara-dark-green');
        } else {
            this.replaceTableTheme('lara-light-green');
        }
    },
    methods: {
        onDarkModeToggle() {
            const newTheme = this.$appState.darkTheme ? 'lara-light-green' : 'lara-dark-green';
            const newTableTheme = this.$appState.darkTheme ? this.tableTheme.replace('dark', 'light') : this.tableTheme.replace('light', 'dark');

            localStorage.setItem(this.$appState.colorSchemeKey, this.$appState.darkTheme ? 'light' : 'dark');
            EventBus.emit('theme-change', { theme: newTheme, dark: !this.$appState.darkTheme });
            this.replaceTableTheme(newTableTheme);
        },
        onTableThemeChange(value) {
            this.replaceTableTheme(value);
        },
        replaceTableTheme(newTheme) {
            const elementId = 'home-table-link';
            const linkElement = document.getElementById(elementId);
            const tableThemeTokens = linkElement?.getAttribute('href').split('/') || null;
            const currentTableTheme = tableThemeTokens ? tableThemeTokens[tableThemeTokens.length - 2] : null;

            if (currentTableTheme !== newTheme && tableThemeTokens) {
                const newThemeUrl = linkElement.getAttribute('href').replace(currentTableTheme, newTheme);

                const cloneLinkElement = linkElement.cloneNode(true);

                cloneLinkElement.setAttribute('id', elementId + '-clone');
                cloneLinkElement.setAttribute('href', newThemeUrl);
                cloneLinkElement.addEventListener('load', () => {
                    linkElement.remove();
                    cloneLinkElement.setAttribute('id', elementId);
                });
                linkElement.parentNode?.insertBefore(cloneLinkElement, linkElement.nextSibling);

                this.tableTheme = newTheme;
            }
        }
    },
    computed: {
        landingClass() {
            return ['landing', { 'layout-dark': this.$appState?.darkTheme, 'layout-light': !this.$appState?.darkTheme, 'layout-news-active': this.$appState?.newsActive }];
        }
    },
    components: {
        AppNews,
        AppTopBar,
        HeroSection,
        ThemeSection,
        BlockSection,
        TemplateSection,
        UsersSection,
        FeaturesSection,
        FooterSection
    }
};
</script>
