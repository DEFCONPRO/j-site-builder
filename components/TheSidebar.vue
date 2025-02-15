<template>
  <aside
    data-cy="sidebar__container"
    class="sidebar grid h-screen max-w-[260px] grid-rows-[auto,auto,1fr,auto] gap-8 bg-green-600 p-6"
  >
    <section ref="sidebar-title" data-cy="sidebar__header" class="flex gap-2">
      <img
        src="/logos/logo.svg"
        alt="Portal Jabar Logo"
        width="34"
        height="34"
        class="self-start"
      />
      <div>
        <img
          src="/logos/logo-text.svg"
          alt="Portal Jabar Logo"
          width="75"
          height="38"
          class="mb-1"
        />
        <h1
          data-cy="sidebar__header-title"
          class="whitespace-nowrap font-lato text-sm font-bold leading-5 text-white"
        >
          CONTENT MANAGEMENT<br />
          SYSTEM
        </h1>
      </div>
    </section>

    <!-- Site Selection -->
    <SidebarSiteSelect />

    <nav
      class="sidebar__navigation-container relative right-6 -mx-6 w-[calc(100%+48px)] overflow-y-auto"
    >
      <ul data-cy="sidebar__navigation" class="pl-12 pr-2">
        <li>
          <NuxtLink
            v-for="navigation in NAVIGATION_MENU"
            :key="navigation.label"
            :to="navigation.link"
            class="sidebar__navigation-item mb-2 flex min-h-[50px] items-center rounded-lg p-[15px] font-lato text-sm font-bold text-white last-of-type:mb-0 hover:bg-green-700"
            :data-cy="`j-site-sidebar__button-menu-${navigation.label
              .toLowerCase()
              .replaceAll(' ', '-')}`"
          >
            <NuxtIcon :name="navigation.icon" filled class="text-xl" />
            <span class="ml-3">{{ navigation.label }}</span>
          </NuxtLink>
        </li>
      </ul>
    </nav>
    <section class="mt-auto">
      <ul ref="sidebar-bottom-nav" data-cy="sidebar__bottom-nav">
        <li>
          <NuxtLink
            :to="config.public.portalJabarCMSBaseURL"
            class="mb-2 flex min-h-[50px] w-full items-center rounded-lg p-[15px] font-lato text-sm font-bold text-white hover:bg-green-700"
            data-cy="j-site-sidebar__button-menu-cms-portal-jabar"
          >
            <NuxtIcon
              name="navigation/portal-cms-icon"
              filled
              class="text-xl"
            />
            <span class="ml-3">CMS Portal Jabar</span>
          </NuxtLink>
        </li>
        <li>
          <NuxtLink
            :to="`${config.public.portalJabarCMSBaseURL}/pengaturan`"
            class="mb-2 flex min-h-[50px] w-full items-center rounded-lg p-[15px] font-lato text-sm font-bold text-white hover:bg-green-700"
          >
            <NuxtIcon
              name="navigation/account-settings-icon"
              filled
              class="text-xl"
            />
            <span class="ml-3">Pengaturan Akun</span>
          </NuxtLink>
        </li>
      </ul>
    </section>
  </aside>
</template>

<script setup lang="ts">
  import { NAVIGATION_MENU } from '~/common/constant/navigation'

  const { $jSiteApi } = useNuxtApp()
  const config = useRuntimeConfig()
  const siteStore = useSiteStore()

  const { data: sites, error } = await $jSiteApi.settings.getSettings(
    undefined, // no query params for this request
    { server: false },
  )

  if (error.value) {
    // @todo: add toast or error page if fetching failed
    console.error(error.value)
  }

  siteStore.sites = sites.value?.data || null
</script>

<style scoped>
  .sidebar__navigation-item.router-link-exact-active {
    @apply bg-green-700;
  }

  .sidebar__navigation-container::-webkit-scrollbar {
    width: 4px;
  }

  .sidebar__navigation-container::-webkit-scrollbar-track {
    background-color: none;
  }

  .sidebar__navigation-container::-webkit-scrollbar-thumb {
    background-color: #9bdbb3;
    outline: none;
    border-radius: 6px;
    background-clip: padding-box;
  }
</style>
