<template>
    <aside>
        <ul v-for="link in navLinks" class="">
            <li>
                <i :class="link.icon"></i>
                <a :href="link.url">
                    {{ link.content }}
                </a>
            </li>
        </ul>
    </aside>
</template>

<script>
export default {
    data() {
        return {
            navLinks: [
                {
                    content: 'Dashboard',
                    icon: 'fa-solid fa-table',
                    url: '#',
                    // children: [
                    //     {
                    //         content: 'Home-1',
                    //         url: '#',
                    //     },
                    //     {
                    //         content: 'Home-2',
                    //         url: '#',
                    //     },
                    // ]
                },
                {
                    content: 'About',
                    icon: 'fa-solid fa-chart-simple',
                    url: '#',
                    // children: [
                    //     {
                    //         content: 'Home-1',
                    //         url: '#',
                    //     },
                    //     {
                    //         content: 'Home-2',
                    //         url: '#',
                    //     },
                    // ]
                },
            ],
        }
    },
}
</script>

<style lang="scss" scoped>
@use "../styles/partials/variables" as *;
@use "../styles/partials/mixins" as *;
    li {
        @include flex(row, start, center);
        gap: 1rem;
    }
</style>