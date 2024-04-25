<script>
import AppHeader from "@/components/AppHeader.vue";
export default {
    components: {
        AppHeader,
    },
};
</script>

<template>
    <div id="app">
        <AppHeader />
        <div class="container mt-3">
            <router-view />
        </div>
    </div>
</template>

<style scoped>

#body{
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

</style>