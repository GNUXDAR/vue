<template>
    <header class="shadow w-screen">
        <nav>
            <nav class="flex items-center justify-between flex-wrap bg-green-400 p-6">
                <div class="flex items-center flex-shrink-0 text-white mr-6">
                    <art-my-icon  class="mr-2"/> <!-- componente del icon -->
                    <span class="font-semibold text-xl tracking-tight">
                        <router-link
                            :to="{ name: 'home'}"> VUE Exchange </router-link>
                    </span>
                    <span class="ml-2">
                        <router-link
                        to="/about">About</router-link>
                    </span>
                </div>
                <div class="hidden sm:block w-full block flex-grow lg:flex lg:items-center lg:w-auto">
                    <div class="text-sm lg:flex-grow"></div>
                </div>
            </nav>
        </nav>
    </header>
</template>

<script>
import ArtMyIcon from '@/components/ArtMyIcon.vue'
export default {
    name: 'ArtHeader',
    components: { ArtMyIcon }

}
</script>