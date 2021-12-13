<template>
<div class="hello">
    <div>
        <v-app-bar color="#004890" dense dark>
        </v-app-bar>
    </div>
    <CardChecks></CardChecks>
</div>
</template>

<script>

import CardChecks from "./CardChecks.vue"

export default {
    name: 'HelloWorld',
    components: {
        CardChecks,
    }
}
</script> 

<style scoped>
</style>
