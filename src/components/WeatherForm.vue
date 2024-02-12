<template>

<form @submit.prevent>
        <h2>Погода</h2>

        <my-input 
        v-model="item.city" 
        type="text" 
        placeholder="Введите название города"/>
        <button @click="createItem" class="button">Добавить</button>

</form>
    
</template>

<script>


export default {
    data () {
        return {
            item: {
                city: '',
            }
        }
    },
    methods: {
        createItem() {
            this.item.id = Date.now();
            this.$emit('create', this.item);
            this.item = {
                city: '',
            }
        }
    }
}
</script>

<style scoped>

form {
    display: flex;
    flex-direction: column;  
    width: 70%;
    margin-top: 15px;
    margin: 0 auto;
}

.button {
    padding: 10px 15px;
    background: rgb(128,128,128);
    background: radial-gradient(circle, rgba(122,134,194,1) 0%, rgba(128,128,128,1) 100%);
    border: 2px solid grey;
    width: 20%;
    border-bottom-right-radius: 10px;
    border-bottom-left-radius: 10px;
    }

h2 {
    color: white;
    font-family: Arial
    }; 

    








</style>