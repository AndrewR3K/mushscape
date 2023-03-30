<template>
    <div>
        <!-- this checkbox controls if drawer is open -->
        <input id="menu-drawer" type="checkbox" class="drawer-toggle" />
        <div class="flex flex-col drawer-content">
            <!-- drawer content -->
            <div class="w-full navbar bg-base-300">
                <!-- hamburger menu is only visible on mobile -->
                <div class="flex-none lg:hidden">
                    <label for="menu-drawer" class="btn btn-square btn-ghost">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                            class="inline-block w-6 h-6 stroke-current">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M4 6h16M4 12h16M4 18h16"></path>
                        </svg>
                    </label>
                </div>
                <div class="flex-1 px-2 mx-2 font-bold">
                    <span>
                        <div class="w-16 h-16 mask mask-circle">
                            <img src="/logo.png" style="pointer-events: none" />
                        </div>
                    </span>
                </div>
                <div class="flex-none">
                    <!-- navbar is only visible for desktop -->
                    <div class="hidden lg:inline-block">
                        <ul class="mr-2 space-x-2 menu horizontal">
                            <li>
                                <a target="_blank" href="https://github.com/AndrewR3K/mushscape" class="rounded-btn">Github</a>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
            <!-- main content -->
            <div class="grid flex-grow align-center self-center place-content-center max-w-md">
                <slot></slot>
            </div>
        </div>
        <!-- drawer sidebar for mobile -->
        <div class="drawer-side">
            <label for="menu-drawer" class="drawer-overlay"></label>
            <ul class="p-4 overflow-y-auto menu w-80 bg-base-100">
                <li>
                    <a target="_blank" href="https://github.com/AndrewR3K/mushscape">Github</a>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    name: 'HeaderComp',
};
</script>

