<script>
    import Chat from "./Chat.svelte";
    import Login from "./Login.svelte";

    let inChat = false;
    let userName = '';

    function toChat(ev) {
        userName = ev.detail;
        inChat = true;
    }

    function toLogin(ev) {
        userName = ev.detail;
        inChat = false;
    }
</script>

<div class="body">
    <div class="body-content">
        {#if inChat}
            <Chat {userName} on:exitToLogin={toLogin}/>
        {:else}
            <Login {userName} on:LoginEntered={toChat}/>
        {/if}
    </div>
</div>

<style>
    .body {
        background: rgb(24,19,110);
        background: radial-gradient(circle, rgba(24,19,110,1) 0%, rgba(29,29,173,1) 29%, rgba(18,139,164,1) 77%);
        height: calc(100vh - 94px);
    }
    .body-content {
        max-width: 520px;
        margin: 0 auto;
        height: 100%;
    }
</style>