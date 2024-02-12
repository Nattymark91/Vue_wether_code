<template>
<input :value="modelValue" @input="updateInput" class="input" type="text">
</template>

<script>
export default {
    name: 'my-input',
    props: {
        modelValue: [String, Number]
    },
    methods: {
        updateInput(event) {
            this.$emit('update:modelValue', event.target.value)
        }
    }
}

</script>

<style scoped>
.input { 
    padding: 10px;
    border: 2px solid gray;
    margin-top: 15px;
    background-color: black;
    color: white;
    width: 68.5vw;
    border-bottom-right-radius: 10px;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
}
</style>