<template>
  <div class="app max-w-[29rem] w-full p-4 sm:py-12 h-screen flex flex-col">
    <router-view/>
    <Footer v-if="layout.footer"/>
  </div>
</template>

<script>
  import Header from "@/components/Layout/Header";
  import Footer from "@/components/Layout/Footer";

  export default {
    metaInfo() {
      let langList = []
      this.$i18n.availableLocales.forEach(lang => {
        langList.push({
          rel: "alternate",
          href: `https://discordlookup.vercel.app/${lang !== this.$i18n.locale ? ('?hl=' + lang) : ""}`,
          hreflang: lang
        })
      })
      return {
        meta: [
          ...langList
        ]
      }
    },
    data() {
      return {
        scrollTop: 0,
        bars: false,
        layoutBase: {
          header: false,
          footer: true
        },
        layout: {}
      }
    },
    created() {
      let that = this;

      // Set Layout Elements
      this.layout = { ...this.layoutBase }

      // Set Screen Size
      that.screenSize();
      window.addEventListener('resize', function () {
        that.screenSize();
      });

      // Detect iPhone Bars
      let iPhone = /iPhone/.test(navigator.userAgent) && !window.MSStream
      let aspect = window.screen.width / window.screen.height
      if (iPhone && aspect.toFixed(3) === "0.462") {
        that.bars = true;
      }
    },
    components: {
      Header,
      Footer
    },
    methods: {
      screenSize() {
        document.documentElement.style.setProperty('--app-height', `${document.documentElement.clientHeight}px`)
        document.documentElement.style.setProperty('--app-width', `${document.documentElement.clientWidth}px`)
      }
    }
  }
</script>

<style lang="scss">
  @tailwind base;
  @tailwind components;
  @tailwind utilities;

  // Body
  body {
    background: #f3f4f6;
    min-height: var(--app-height, 100vh);

    @apply h-full flex flex-col justify-center items-center text-gray-800;
  }

  // Components
  .tooltip {
    @apply bg-gray-200 bg-opacity-50 border border-gray-100 mt-1 px-2 py-1 rounded-md backdrop-filter backdrop-blur-sm text-xs font-bold;

    &__text {
      @apply font-normal;
    }
  }
</style>
