<script setup>

defineProps({
    name: String,
    message: String,
    isRight: Boolean
})

</script>
<template>
    <div class="message">

        <label class="name" :class="{ 'message-right': !isRight, 'message-left': isRight }">{{ name }}</label>
        <div :class="{ 'message-right': !isRight, 'message-left': isRight }">
            <span class="bubble" :class="{ 'bg-left': !isRight, 'bg-right': isRight }">{{ message }}</span>
        </div>
    </div>
</template>

<style scoped>
.message {
    display: flex;
    flex-direction: column;
    width: 100%;
    padding: 5px 15px;
    font-family: Arial, Helvetica, sans-serif;
}

.name {
    font-size: 14px;
    margin-bottom: 2px;
}

.bubble {
    display: inline-block;
    width: fit-content;
    padding: 5px 10px;
    border-radius: 5px;
}

.message-left {
    text-align: left;
}

.message-right {
    text-align: right;
}

.bg-left {
    background-color: rgb(219, 149, 240);
}

.bg-right {
    background-color: rgb(81, 219, 219);
}
</style>