<script>
    export let isMine;
    export let msg;
</script>

<div class='message' class:mine={isMine}>
    <div class="user-name">{msg.userName}</div>
    <div class="message-text">{msg.text}</div>
</div>

<style>
    .message {
        max-width: 70%;
        background-color: white;
        width: max-content;
        padding: 6px 10px;
        display: flex;
        flex-direction: column;
        gap: 4px;
        font-size: 12px;    
        border-radius: 4px;
    }

    .message.mine {
        background: rgb(87, 180, 87);
        margin-left: auto;
    }

    .message-text {
       text-overflow: ellipsis; 
       overflow-x: hidden;
    }

    .user-name {
        font-size: 10px;
        font-weight: 600;
        color: pink;
        user-select: none;
    }
</style>