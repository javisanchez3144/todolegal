<template>
    <nav class="navbar">
        <div class="navbar-left">
            <img src="../assets/logo-todo-legal.webp" alt="Left Logo" class="logo-left" />
        </div>
        <div class="navbar-right">
            <span class="navbar-text">
                Smart <span class="blue-text">Contracts</span> Suite
            </span>
            <div class="menu-spacer"></div>
            <div class="menu-icon">
                <i class="fa fa-bars"></i>
            </div>
        </div>
        <div :class="['menu', { 'menu-open': isMenuOpen }]">
        </div>
    </nav>
</template>
<script>
export default {
    data() {
        return {
            isMenuOpen: false
        };
    },
    methods: {
        toggleMenu() {
            this.isMenuOpen = !this.isMenuOpen;
        }
    }
};
</script>

<style scoped>
@import "../assets/css/SiteNavbar.css";
</style>  