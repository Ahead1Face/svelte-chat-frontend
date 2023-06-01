<script>
    import { createEventDispatcher } from 'svelte';

    const despatch = createEventDispatcher();
    export let userName = ''

    function submit() { 
        if (userName) {
            despatch('LoginEntered', userName);
        }
    }
</script>

<form class='login' on:submit|preventDefault={submit}>
    <input type="text" bind:value={userName} placeholder="Имя">
    <button type='submit'>В чат!</button>
</form>

<style>
    .login {
        padding: 100px 15px;
        display: flex;
        flex-direction: column;
        gap: 30px;
    }
    input {
        height: 40px;
        border: 0;
        outline: 0;
        background-color: white;
        padding: 0 12px;
        border-radius: 2px;
    }
    input:focus {
        border: rgb(67, 67, 216);
    }
    button {
        height: 40px;
        border: 0;
        outline: 0;
        background: rgb(67, 67, 216);
        color: white;
        width: 100%;
        transition: all .2s ease-out;
        border-radius: 2px;
    }

    button:hover {
        transform: scale(1.1);
    }
</style>