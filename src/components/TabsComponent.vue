<template>
    <div class="tabs">
        <div class="tabs-header flex items-center justify-between">
            <div class="flex items-center">

                <div class="title">{{ pageTitle }}</div>
            </div>
            <div class="tab-icons">
                <div v-for="(tab, index) in tabs" :key="index" class="tab-icon" :class="{ 'active': activeTab === index }"
                    @click="changeTab(index)">
                    <span :class="['tab-number', { 'active': activeTab === index }]" :key="index">
                        {{ tab.number }}
                    </span>{{ tab.title }}
                </div>
            </div>
        </div>

    </div>
    <!-- <div class="tabs-content">
        <slot :activeTab="activeTab"></slot>
    </div> -->
</template>
  
<script>
export default {
    props: {
        pageTitle: String,
        tabs: Array,
    },
    data() {
        return {
            activeTab: 0,
        };
    },
    methods: {
        changeTab(index) {
            this.activeTab = index;
        },
    },
};
</script>
  
<style scoped>
@import "../assets/css/TabsComponent.css";
</style>