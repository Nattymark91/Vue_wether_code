<template>
<button class="btn">
<img src="@/icons/magnifier.png" alt="bin" width="20px">
<slot></slot>

</button>

    
</template>

<script>
export default {
    name: 'show-button'
}

</script>

<style scoped>

.btn {
    align-self: flex-end;
    padding: 5px;
    background-color: white;
    border: 1px solid;
    border-radius: 10px;
}
</style>