<script setup>
import { ref } from 'vue';
const value = ref('');

const emit = defineEmits(['newBoard'])
const label = defineProps({
    label: String
})

const handleInput = () => {
    if(value.value != '') emit('newBoard', value.value);
    value.value = ''
};
</script>

<template>
    <form @submit.prevent="handleInput" class="container-input">
        <div class="box-input">
            <v-text-field class="input" rounded :label="label.label" persistent-hint variant="outlined" v-model="value"
        ></v-text-field>
        </div>
        
        <div class="box-button">
            <button class="button">crear</button>
        </div>
        
    </form>
</template>

<style scoped>
.container-input {
    display: flex;
    padding: 0.5em 0;
}
.button{
    height: 100%;
    width: 100%;
    background-color: #282E33;
}
.box-input{
    height: 100%;
    border-radius: 10px;
    width: 80%;
}
.input{
    height: 3.5em;
    border-radius: 25px;
}
.box-button{
    height: 100%;
    width: 20%;
    height: 3.5em;
}
@media (max-width: 455px){
    .box-input{
        width: 70%;
    }
    .box-button{
        width: 30%;
    }
}
</style>
