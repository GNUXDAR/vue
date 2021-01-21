<template>
    <div>
        <art-assets-table  v-bind:assets="assets" />
    </div>
</template>

<script>
import api from '@/api'
import ArtAssetsTable from '@/components/ArtAssetsTable.vue';

export default {
    name: 'Home',
    components: {
        ArtAssetsTable,
    },

    data(){
        return {
            assets: []
        }
    },

    created() {
        api.getAssets()
            .then(assets => (this.assets = assets))
    }
}
</script>