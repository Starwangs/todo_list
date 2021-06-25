<script>
    import { fade, scale } from "svelte/transition";

    export let displayName;
    export let photoURL;
    export let uid;
</script>

<div
    class="relative max-w-xl w-full h-36 bg-white rounded-lg shadow-lg overflow-hidde mb-32"
>
    <div
        class="absolute inset-0 rounded-lg overflow-hidden bg-red-200 add-background"
        transition:scale={{ delay: 300 }}
    >
        <img
            src="https://images.unsplash.com/photo-1618627575331-b5f0d4cd72ce?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=800&h=200&q=80"
            width="800px"
            height="200px"
            alt=""
        />
        <div
            class="absolute inset-0 backdrop backdrop-blur-10 bg-gradient-to-b from-transparent to-black"
        />
    </div>
    <div class="absolute flex space-x-6 transform translate-x-6 translate-y-8">
        <div
            class="w-36 h-36 rounded-lg shadow-lg overflow-hidden"
            transition:scale={{ delay: 600 }}
        >
            <img src={photoURL} width="200px" height="200px" alt="" />
        </div>
        <div class="text-white pt-12">
            <h3 class="font-bold" transition:scale={{ delay: 400 }}>
                {displayName}
            </h3>
            <div class="text-sm opacity-60" transition:scale={{ delay: 500 }}>
                {uid}
            </div>
            <div class="mt-8 text-gray-400">
                <div class="flex items-center space-x-2 text-xs">
                    <svg
                        class="w-4 h-4"
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        transition:scale={{ delay: 600 }}
                        ><path d="M3 18v-6a9 9 0 0 1 18 0v6" /><path
                            d="M21 19a2 2 0 0 1-2 2h-1a2 2 0 0 1-2-2v-3a2 2 0 0 1 2-2h3zM3 19a2 2 0 0 0 2 2h1a2 2 0 0 0 2-2v-3a2 2 0 0 0-2-2H3z"
                        /></svg
                    >
                    <span transition:scale={{ delay: 800 }}>Easy listening</span
                    ><slot />
                </div>
            </div>
        </div>
    </div>
</div>

<style></style>
