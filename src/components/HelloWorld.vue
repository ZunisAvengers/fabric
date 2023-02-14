<script setup>
import { onMounted } from "vue";
const accessToken = ref("");
const redirectUrl = ref("");

onMounted(() => {
    const url = new URL(document.URL);
    if (url.searchParams.has("access_token ")) {
        accessToken.value = url.searchParams.get("access_token");
    }

    const redirect = new URL("https://www.tiktok.com/auth/authorize/");
    url.searchParams.append("client_key", "aw70ajbbxwn1leb6");
    url.searchParams.append("scope", "");
    url.searchParams.append("redirect_uri", "http://localhost:3000/auth");
    url.searchParams.append("state", csrfState);
    url.searchParams.append("response_type", "code");
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
</template>
