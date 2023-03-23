<template>
    <mHeader>
        <div class="w-full max-w-md p-4 text-center">
            <div class="mx-auto avatar">
                <div v-if="promptimage" class="w-256 h-256 mb-8 mask mask-squircle bg-secondary">
                    <img :src="promptimage.url" />
                </div>
                <div v-else-if="!game_started" class="w-44 h-44 mask mask-circle">
                    <img src="/logo.png" style="pointer-events: none" />
                </div>
                <div v-else>
                    <svg aria-hidden="true" class="w-8 h-8 mr-2 text-gray-200 animate-spin text-gray-600 fill-primary"
                        viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path
                            d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z"
                            fill="currentColor" />
                        <path
                            d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z"
                            fill="currentFill" />
                    </svg>
                </div>
            </div>

            <h2 v-if="!game_started" class="py-2 text-4xl font-bold mb-2">MushScape</h2>
            <div v-if="!game_started" class="py-2 text-2xl font-medium mb-8">Enter the world of Mushscape - the AI-powered
                text-based RPG that's full of surprises! Explore a magical universe filled with challenges and unexpected
                twists. Are you ready for the adventure?</div>


            <h2 v-else-if="!gameloading" class="py-2 text-2xl font-bold mb-8">{{ gametitle }}</h2>

            <div v-if="!game_started" class="pb-3">

                    <button class="btn btn-primary inline-flex items-center" @click="startGame">Start Your
                        Adventure</button>

                <div class="w-6 justify-center mx-auto mt-4">
                    <svg xmlns="http://www.w3.org/2000/svg"
                        class=" text-sm leading-5 cursor-pointer text-grey-600 hover:text-secondary cog"
                        @click="showsettings = !showsettings"
                        viewBox="0 0 512 512"><!--! Font Awesome Pro 6.3.0 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. -->
                        <path fill="currentColor"
                            d="M481.9 166.6c3.2 8.7 .5 18.4-6.4 24.6l-30.9 28.1c-7.7 7.1-11.4 17.5-10.9 27.9c.1 2.9 .2 5.8 .2 8.8s-.1 5.9-.2 8.8c-.5 10.5 3.1 20.9 10.9 27.9l30.9 28.1c6.9 6.2 9.6 15.9 6.4 24.6c-4.4 11.9-9.7 23.3-15.8 34.3l-4.7 8.1c-6.6 11-14 21.4-22.1 31.2c-5.9 7.2-15.7 9.6-24.5 6.8l-39.7-12.6c-10-3.2-20.8-1.1-29.7 4.6c-4.9 3.1-9.9 6.1-15.1 8.7c-9.3 4.8-16.5 13.2-18.8 23.4l-8.9 40.7c-2 9.1-9 16.3-18.2 17.8c-13.8 2.3-28 3.5-42.5 3.5s-28.7-1.2-42.5-3.5c-9.2-1.5-16.2-8.7-18.2-17.8l-8.9-40.7c-2.2-10.2-9.5-18.6-18.8-23.4c-5.2-2.7-10.2-5.6-15.1-8.7c-8.8-5.7-19.7-7.8-29.7-4.6L69.1 425.9c-8.8 2.8-18.6 .3-24.5-6.8c-8.1-9.8-15.5-20.2-22.1-31.2l-4.7-8.1c-6.1-11-11.4-22.4-15.8-34.3c-3.2-8.7-.5-18.4 6.4-24.6l30.9-28.1c7.7-7.1 11.4-17.5 10.9-27.9c-.1-2.9-.2-5.8-.2-8.8s.1-5.9 .2-8.8c.5-10.5-3.1-20.9-10.9-27.9L8.4 191.2c-6.9-6.2-9.6-15.9-6.4-24.6c4.4-11.9 9.7-23.3 15.8-34.3l4.7-8.1c6.6-11 14-21.4 22.1-31.2c5.9-7.2 15.7-9.6 24.5-6.8l39.7 12.6c10 3.2 20.8 1.1 29.7-4.6c4.9-3.1 9.9-6.1 15.1-8.7c9.3-4.8 16.5-13.2 18.8-23.4l8.9-40.7c2-9.1 9-16.3 18.2-17.8C213.3 1.2 227.5 0 242 0s28.7 1.2 42.5 3.5c9.2 1.5 16.2 8.7 18.2 17.8l8.9 40.7c2.2 10.2 9.4 18.6 18.8 23.4c5.2 2.7 10.2 5.6 15.1 8.7c8.8 5.7 19.7 7.7 29.7 4.6l39.7-12.6c8.8-2.8 18.6-.3 24.5 6.8c8.1 9.8 15.5 20.2 22.1 31.2l4.7 8.1c6.1 11 11.4 22.4 15.8 34.3zM242 336a80 80 0 1 0 0-160 80 80 0 1 0 0 160z" />
                    </svg>
                </div>

                <Transition name="slide-fade">
                    <div class="mt-4" v-if="showsettings">
                        <mCard>
                            <div class="relative">
                                <label class="label">
                                    <span class="label-text">OpenAI APIKey</span>
                                </label>
                                <input v-model="apikey" placeholder="sk-*********"
                                    class="input input-bordered input-primary w-full max-w-xs"
                                    :type="show ? 'text' : 'password'">
                                <div
                                    class="absolute inset-y-0 right-0 pr-3 flex items-center text-sm leading-5 cursor-pointer">
                                    <svg xmlns="http://www.w3.org/2000/svg" @click="show = !show"
                                        class="h-6 mt-8 text-primary" v-show="!show"
                                        viewBox="0 0 576 512"><!--! Font Awesome Pro 6.3.0 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. -->
                                        <path fill="currentColor"
                                            d="M288 32c-80.8 0-145.5 36.8-192.6 80.6C48.6 156 17.3 208 2.5 243.7c-3.3 7.9-3.3 16.7 0 24.6C17.3 304 48.6 356 95.4 399.4C142.5 443.2 207.2 480 288 480s145.5-36.8 192.6-80.6c46.8-43.5 78.1-95.4 93-131.1c3.3-7.9 3.3-16.7 0-24.6c-14.9-35.7-46.2-87.7-93-131.1C433.5 68.8 368.8 32 288 32zM144 256a144 144 0 1 1 288 0 144 144 0 1 1 -288 0zm144-64c0 35.3-28.7 64-64 64c-7.1 0-13.9-1.2-20.3-3.3c-5.5-1.8-11.9 1.6-11.7 7.4c.3 6.9 1.3 13.8 3.2 20.7c13.7 51.2 66.4 81.6 117.6 67.9s81.6-66.4 67.9-117.6c-11.1-41.5-47.8-69.4-88.6-71.1c-5.8-.2-9.2 6.1-7.4 11.7c2.1 6.4 3.3 13.2 3.3 20.3z" />
                                    </svg>

                                    <svg xmlns="http://www.w3.org/2000/svg" @click="show = !show"
                                        class="h-6 mt-8 text-primary" v-show="show"
                                        viewBox="0 0 640 512"><!--! Font Awesome Pro 6.3.0 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. -->
                                        <path fill="currentColor"
                                            d="M38.8 5.1C28.4-3.1 13.3-1.2 5.1 9.2S-1.2 34.7 9.2 42.9l592 464c10.4 8.2 25.5 6.3 33.7-4.1s6.3-25.5-4.1-33.7L525.6 386.7c39.6-40.6 66.4-86.1 79.9-118.4c3.3-7.9 3.3-16.7 0-24.6c-14.9-35.7-46.2-87.7-93-131.1C465.5 68.8 400.8 32 320 32c-68.2 0-125 26.3-169.3 60.8L38.8 5.1zM223.1 149.5C248.6 126.2 282.7 112 320 112c79.5 0 144 64.5 144 144c0 24.9-6.3 48.3-17.4 68.7L408 294.5c8.4-19.3 10.6-41.4 4.8-63.3c-11.1-41.5-47.8-69.4-88.6-71.1c-5.8-.2-9.2 6.1-7.4 11.7c2.1 6.4 3.3 13.2 3.3 20.3c0 10.2-2.4 19.8-6.6 28.3l-90.3-70.8zM373 389.9c-16.4 6.5-34.3 10.1-53 10.1c-79.5 0-144-64.5-144-144c0-6.9 .5-13.6 1.4-20.2L83.1 161.5C60.3 191.2 44 220.8 34.5 243.7c-3.3 7.9-3.3 16.7 0 24.6c14.9 35.7 46.2 87.7 93 131.1C174.5 443.2 239.2 480 320 480c47.8 0 89.9-12.9 126.2-32.5L373 389.9z" />
                                    </svg>
                                </div>
                            </div>

                            <a class="link link-primary" target="_blank"
                                href="https://platform.openai.com/account/api-keys">(Get
                                your API key here)</a>


                            <div class="mt-4">
                                <label class="label">
                                    <span class="label-text">Tokens</span>
                                </label>
                                <input v-model="tokens" class="input input-bordered input-primary w-full max-w-xs"
                                    :type="'number'">
                            </div>
                        </mCard>
                    </div>
                </Transition>
            </div>
            <div v-else class="pb-3">
                <div v-if="gameloading">
                    <h2 class="py-2 text-2xl font-bold inline-flex items-center">
                        Generating Scene...
                    </h2>
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
    </mHeader>
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

    <mFooter></mFooter>
</template>

<script>
import axios from 'axios'
import axiosRetry from 'axios-retry';

import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
import Card from './components/Card.vue'

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
            tokens: 31,
            errors: [],
            error: '',
            showsettings: false
        };
    },
    components: {
        mHeader: Header,
        mFooter: Footer,
        mCard: Card
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
            this.promptimage = await this.askDE(this.currentPrompt.prompt + 'in the style of Millet of France artwork.')

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
                max_tokens: this.tokens
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
<style scoped>
.slide-fade-enter-active {
    transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
    transition: all 0.3s ease-out;
}

.slide-fade-enter-from,
.slide-fade-leave-to {
    transform: translateY(20px);
    opacity: 0;
}

.cog {
    transition: all 0.4s ease-out;
}

.cog:hover {
    transform: rotate(90deg);
    transition: all 0.4s ease-out;
}
</style>