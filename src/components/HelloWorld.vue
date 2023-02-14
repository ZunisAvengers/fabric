<script setup>
import { onMounted, ref } from "vue";
const accessToken = ref("");
const redirectUrl = ref("");

onMounted(() => {
    const csrfState = Math.random().toString(36).substring(2);
    const url = new URL(document.URL);
    if (url.searchParams.has("access_token ")) {
        accessToken.value = url.searchParams.get("access_token");
    }

    const redirect = new URL("https://www.tiktok.com/auth/authorize/");
    redirect.searchParams.append("client_key", "aw70ajbbxwn1leb6");
    redirect.searchParams.append("scope", "");
    redirect.searchParams.append("redirect_uri", document.baseURI);
    redirect.searchParams.append("state", csrfState);
    redirect.searchParams.append("response_type", "code");
    redirectUrl.value = redirect.toString();
});

function copy() {
    navigator.clipboard.writeText(accessToken.value);
}

function sendRequest() {
    fetch("http://localhost:3000/auth?token=" + accessToken.value);
}
</script>

<template>
    <div>
        <template v-if="accessToken">
            <p>AccessToken:</p>
            <h1>{{ accessToken }}</h1>
            <button @click="copy">Copy</button>
            <button @click="sendRequest">Send Request</button>
        </template>
        <template v-else>
            <h2>Haven't AccessToken</h2>
        </template>
        <a :href="redirectUrl">
            <button>auth</button>
        </a>
    </div>
</template>
