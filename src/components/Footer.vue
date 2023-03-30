<template>
    <footer class="footer footer-center p-10 bg-secondary text-secondary-content">
        <div>
            <p class="font-medium mb-4">
                This site utilized Chat-GPT and Dall-e
            </p>



            <p class="font-bold">
                Site created by <a href="https://github.com/AndrewR3K" target="_blank"
                    class="link link-primary">bytesizd</a>
            </p>
            <p>Copyright © 2023 - All right reserved</p>
            <p><a href="https://github.com/AndrewR3K/mushscape" target="_blank" class="link link-primary">{{ version }}</a></p>
        </div>
    </footer>
</template>

<script>
import pkg from '../../package.json'
export default {
    name: 'FooterComp',
    data() {
        return {
            version: pkg.version || '0.0.0'
        }
    }
};
</script>

<style scoped>
    a {
        text-decoration: none;
    }
</style>