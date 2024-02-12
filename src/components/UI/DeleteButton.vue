<template>
<button class="btn">
<img src="@/icons/bin.png" alt="bin" width="20px">
<slot></slot>

</button>

    
</template>

<script>
export default {
    name: 'delete-button'
}

</script>

<style scoped>

.btn {
    /* margin-top: 15px; */
    align-self: flex-end;
    padding: 5px;
    background-color: red;
    border: 1px solid;
    border-radius: 10px;
}
</style>