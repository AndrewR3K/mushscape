<template>
    <div class="h-screen bg-base-200 drawer text-base-content prevent-select">
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
                                <a target="_blank" href="#" class="rounded-btn">Github</a>
                            </li>
                            <li>
                                <a target="_blank" href="#" class="rounded-btn">Discord</a>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
            <!-- main content -->
            <div class="grid flex-grow place-content-center">
                <div class="w-full max-w-md p-4 text-center">
                    <!-- avatar -->

                    <div class="mx-auto avatar">
                        <div v-if="promptimage" class="w-256 h-256 mb-8 mask mask-squircle bg-secondary">
                            <img :src="promptimage.url" />
                        </div>
                        <div v-else-if="!game_started" class="w-44 h-44 mask mask-circle">
                            <img src="/logo.png" style="pointer-events: none" />
                        </div>
                    </div>

                    <h2 v-if="!game_started" class="py-2 text-2xl font-bold mb-2">MushScape</h2>
                    <div v-if="!game_started" class="py-2 text-2xl font-medium mb-8">Enter the world of Mushscape - the AI-powered text-based RPG that's full of surprises! Explore a magical universe filled with challenges and unexpected twists. Are you ready for the adventure?</div>


                    <h2 v-else-if="!gameloading" class="py-2 text-2xl font-bold mb-8">{{ gametitle }}</h2>

                    <div v-if="!game_started" class="pb-3">

                        <button class="btn btn-primary" @click="startGame">Start Your Adventure</button>
                        <div class="relative">
                            <input v-model="apikey" placeholder="OpenAI API Key"
                                class="input input-bordered input-primary w-full max-w-xs mt-10"
                                :type="show ? 'text' : 'password'">

                            <div class="absolute inset-y-0 right-0 pr-3 flex items-center text-sm leading-5 cursor-pointer">
                                <svg xmlns="http://www.w3.org/2000/svg" @click="show = !show" class="h-6 mt-10 text-primary"
                                    v-show="!show"
                                    viewBox="0 0 576 512"><!--! Font Awesome Pro 6.3.0 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. -->
                                    <path fill="currentColor"
                                        d="M288 32c-80.8 0-145.5 36.8-192.6 80.6C48.6 156 17.3 208 2.5 243.7c-3.3 7.9-3.3 16.7 0 24.6C17.3 304 48.6 356 95.4 399.4C142.5 443.2 207.2 480 288 480s145.5-36.8 192.6-80.6c46.8-43.5 78.1-95.4 93-131.1c3.3-7.9 3.3-16.7 0-24.6c-14.9-35.7-46.2-87.7-93-131.1C433.5 68.8 368.8 32 288 32zM144 256a144 144 0 1 1 288 0 144 144 0 1 1 -288 0zm144-64c0 35.3-28.7 64-64 64c-7.1 0-13.9-1.2-20.3-3.3c-5.5-1.8-11.9 1.6-11.7 7.4c.3 6.9 1.3 13.8 3.2 20.7c13.7 51.2 66.4 81.6 117.6 67.9s81.6-66.4 67.9-117.6c-11.1-41.5-47.8-69.4-88.6-71.1c-5.8-.2-9.2 6.1-7.4 11.7c2.1 6.4 3.3 13.2 3.3 20.3z" />
                                </svg>


                                <svg xmlns="http://www.w3.org/2000/svg" @click="show = !show" class="h-6 mt-10 text-primary"
                                    v-show="show"
                                    viewBox="0 0 640 512"><!--! Font Awesome Pro 6.3.0 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. -->
                                    <path fill="currentColor"
                                        d="M38.8 5.1C28.4-3.1 13.3-1.2 5.1 9.2S-1.2 34.7 9.2 42.9l592 464c10.4 8.2 25.5 6.3 33.7-4.1s6.3-25.5-4.1-33.7L525.6 386.7c39.6-40.6 66.4-86.1 79.9-118.4c3.3-7.9 3.3-16.7 0-24.6c-14.9-35.7-46.2-87.7-93-131.1C465.5 68.8 400.8 32 320 32c-68.2 0-125 26.3-169.3 60.8L38.8 5.1zM223.1 149.5C248.6 126.2 282.7 112 320 112c79.5 0 144 64.5 144 144c0 24.9-6.3 48.3-17.4 68.7L408 294.5c8.4-19.3 10.6-41.4 4.8-63.3c-11.1-41.5-47.8-69.4-88.6-71.1c-5.8-.2-9.2 6.1-7.4 11.7c2.1 6.4 3.3 13.2 3.3 20.3c0 10.2-2.4 19.8-6.6 28.3l-90.3-70.8zM373 389.9c-16.4 6.5-34.3 10.1-53 10.1c-79.5 0-144-64.5-144-144c0-6.9 .5-13.6 1.4-20.2L83.1 161.5C60.3 191.2 44 220.8 34.5 243.7c-3.3 7.9-3.3 16.7 0 24.6c14.9 35.7 46.2 87.7 93 131.1C174.5 443.2 239.2 480 320 480c47.8 0 89.9-12.9 126.2-32.5L373 389.9z" />
                                </svg>
                            </div>
                        </div>
                        <div class="mt-4">
                            <a class="link link-primary" target="_blank"
                                href="https://platform.openai.com/account/api-keys">(Get your API key here)</a>
                        </div>
                    </div>
                    <div v-else class="pb-3">
                        <div v-if="gameloading">
                            <h2 class="py-2 text-2xl font-bold">Generating Scene...</h2>
                        </div>
                        <div v-else>
                            <h2 class="py-2 text-2xl font-bold">{{ currentPrompt.prompt }}</h2>


                            <div class="text-sm text-opacity-70 text-base-content"
                                v-for="(response, index) in currentPrompt.responses" :key="index">
                                <button class="btn btn-secondary mt-6" @click="respondToPrompt(response)">{{
                                    response }}</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <!-- drawer sidebar for mobile -->
        <div class="drawer-side">
            <label for="menu-drawer" class="drawer-overlay"></label>
            <ul class="p-4 overflow-y-auto menu w-80 bg-base-100">
                <li>
                    <a target="_blank" href="#">Github</a>
                </li>
                <li>
                    <a target="_blank" href="#">Discord</a>
                </li>
            </ul>
        </div>
    </div>
    <div class="toast toast-end toast-middle" v-if="errors && errors.length > 0">
        <div class="alert alert-error">
            <div>
                <span>Oops! It looks like there was a hiccup. Try a new story adventurer.</span>
            </div>
        </div>
    </div>
    <div class="toast toast-end toast-middle" v-if="error && error !== ''">
        <div class="alert alert-error">
            <div>
                <span>{{ error }}</span>
            </div>
        </div>
    </div>

    <footer class="footer footer-center p-10 bg-secondary text-secondary-content">
        <div>
            <p class="font-medium mb-4">
                This site utilized Chat-GPT and Dall-e
            </p>



            <p class="font-bold">
                Site created by <a class="link link-primary">bytesizd</a>
            </p>
            <p>Copyright © 2023 - All right reserved</p>
        </div>
    </footer>
</template>

<script>
import axios from 'axios'
import axiosRetry from 'axios-retry';
export default {
    name: 'App',
    data() {
        return {
            show: false,
            defaultimage: null,
            gameloading: false,
            game_started: false,
            gametitle: '',
            openai: null,
            currentCharacter: 'player',
            currentPrompt: {},
            promptimage: null,
            apikey: '',
            errors: [],
            error: ''
        };
    },
    methods: {
        async startGame() {
            this.errors = []
            this.error = ''

            if (!this.apikey) {
                this.error = 'An API Key is required to proceed.'
                return
            }


            this.gameloading = true
            this.currentCharacter = 'player';
            this.game_started = true

            await this.generateScenerio()
            await this.generateTitle()
            this.generateDEImage()


            await this.GenerateResponseList(3)

            this.gameloading = false

        },
        async generateDEImage() {
            console.log("generating Images")
            this.promptimage = await this.askDE(this.currentPrompt.prompt + 'in the style of retri artwork.')

            if (!this.defaultimage) {
                this.defaultimage = await this.askDE(this.gametitle + 'in the style of vector artwork.')
            }

        },
        async askDE(question) {
            const client = axios.create({
                headers: {
                    Authorization: "Bearer " + this.apikey,
                },
                timeout: 30000,
                maxBodyLength: Infinity,
            });

            let img = await client
                .post("https://api.openai.com/v1/images/generations", {
                    prompt: question,
                    n: 1,
                    size: "256x256"
                }).then((response) => {
                    return response.data.data
                })
                .catch((error) => {
                    console.log(error);
                    return []
                });

            return img[0]
        },
        async askGPT(question) {
            console.log("Asking Question")
            axiosRetry(axios, { retries: 3 });
            const client = axios.create({
                headers: {
                    Authorization: "Bearer " + this.apikey,
                },
                timeout: 30000,
                maxBodyLength: Infinity,
            });
            const params = {
                prompt: question,
                model: "text-davinci-003",
                max_tokens: 31
            };
            let rsp = await client
                .post("https://api.openai.com/v1/completions", params)
                .then((result) => {
                    console.log("Asking Recieved")
                    return result
                })
                .catch((err) => {
                    console.log(err);
                    this.errors.push(err.message)
                });
            let split = rsp.data.choices[0].text.split('.')
            if (split.length > 1) {
                split.pop()
            }
            return split.join('.')
        },
        async generateTitle() {
            const response = await this.askGPT(`Create a title for a text based rpg thats first prompt is "${this.currentPrompt.prompt}"`)
            this.gametitle = response
        },
        async generateScenerio() {
            const response = await this.askGPT(`Create a scenerio for the start of a text based adventure that concludes with a question for a player`)
            this.currentPrompt = {
                prompt: response,
                responses: []
            }
        },
        async GenerateResponseList(amt) {
            for (let index = 0; index < amt; index++) {
                this.delay(1000 * index)
                const prompt = `The character ${this.currentCharacter} responds to the prompt "${this.currentPrompt.prompt}" with a message and action to progress through the text based game. Must be in first person.`;
                this.currentPrompt.responses.push(await this.askGPT(prompt))
            }
        },
        async respondToPrompt(response) {
            this.gameloading = true

            const responseprompt = `Create a new prompt for a text based story where the last prompt was "${this.currentPrompt.prompt}" and the character ${this.currentCharacter} just responded to the prompt with "${response}". This new prompt should end with a goal or objective to progress the character ${this.currentCharacter} through the text based game. If you see fit, you can add story, and potentially an ending.`;
            const rsp = await this.askGPT(responseprompt)
            this.currentPrompt = {
                prompt: rsp,
                responses: []
            }


            this.promptimage = null
            this.generateDEImage()

            await this.GenerateResponseList(3)
            this.gameloading = false
        },
        async delay(ms) {
            return new Promise(resolve => setTimeout(resolve, ms));
        },
    },
};
</script>

