<script>
    import { createEventDispatcher, onDestroy, tick } from "svelte";
    import Massage from "./Massage.svelte";
    
    const despatch = createEventDispatcher();
    
    export let userName;

    let data = null;
    let newMessage = '';
    let messages = null;
    let chatBody = null;
    let countMsg = {};

    let handler = { 
        set: function(countMsg, key, value) {
            countMsg[key] = value;
            console.log('изменение сообщений ' + proxy.value);
            load();
            
            return true;
        }
    };
    let proxy = new Proxy(countMsg, handler);
    
    let ngrok = 'https://d32e-79-136-232-236.ngrok-free.app';
    let notNgrok = `//192.168.0.141:13531`;

    load(true);

    async function load(firstLoad = false) {
        try {
            const resp = await fetch(`${notNgrok}/api/messages_get`,
            {
                method: 'POST',
            });
            data = await resp.json();
            messages = data.messages;
            proxy.value = data.countMessage;
        } catch (er) {
            console.log(er);
            alert(er.message);
            return;
        }
        if (firstLoad) {
            await tick();
            chatBody.scrollTop = chatBody.scrollHeight;
        }
    }

    async function submit() {
        if (!newMessage) { return };
        try {
            const resp = await fetch(`${notNgrok}/api/messages_add`, {
                method: 'POST',
                headers: {
                    'Accept': 'application/json',
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    userName,
                    text: newMessage,
                }),
            });
            await resp.json();
        } catch (er) {
            console.log(er);
            alert(er.message);
            return;
        }
        newMessage=''
        await load();
        await tick();
        chatBody.scrollTop = chatBody.scrollHeight;
    }

    function enterKey(ev) {
        if (ev.key === 'Enter') {
            submit();
            ev.preventDefault();
        }
    }

    function exit(ev) {
        despatch('exitToLogin', userName);
        ev.preventDefault();
    }

</script>

<div class="chat">
    <div class="chat-body" bind:this={chatBody}>
        {#if messages == null}
            Загрузка...
        {:else}
            {#each messages as msg (msg.id)}
                <Massage
                    isMine={msg.userName === userName}
                    msg={msg}
                />
            {/each}
        {/if}
    </div>
    <form class="bottom" on:submit|preventDefault={submit}>
        <button class="exit" on:click={exit}>exit</button>
        <textarea bind:value={newMessage} placeholder="Сообщение" on:keydown={enterKey}/>
        <button class='send'>send</button>
    </form>
</div>

<style>
    .chat {
        height: 100%;
        display: flex;
        flex-direction: column;
    }
    
    .chat-body {
        flex: 1 1 auto;
        display: flex;
        flex-direction: column;
        gap: 3px;
        overflow: auto;
        padding: 4px
    }

    .bottom {
        flex: 0 0 auto;
        display: flex;
    }

    textarea {
        border: 0;
        outline: 0;
        resize: none;
        flex: 1 1 auto;
        padding: 12px;
        font-size: 18px;
        
    }

    button { 
        width: 100%;
        border: 0;
        outline: 0;
        background-color: white;
        flex: 0 0 50px;
    }

    .send {
        border-top-right-radius: 4px;
        border-left: 1px solid gray;
    }

    .exit {
        border-top-left-radius: 4px;
        border-right: 1px solid gray;
    }
</style>