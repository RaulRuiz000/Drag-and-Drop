<script setup>
import { ref } from 'vue';
import { mdiCheckCircle, mdiCloseCircle } from '@mdi/js'
import SvgIcon from '@jamescoyle/vue-icon'
const text = ref('')
const cancel = ref(mdiCloseCircle)
const check = ref(mdiCheckCircle)
const emit = defineEmits(['valueEditTask'])

const oldValue = defineProps({
    titleTask: String
})
text.value = oldValue.titleTask

const handleInput = (value) => {
    emit('valueEditTask', {edit: value, content: text.value})
};
</script>
<template>
    <form class="container-input">
        <div class="box-input">
            <v-text-field label="Editar" class="input" persistent-hint variant="outlined" v-model="text"></v-text-field>
        </div>
        
        <div class="container-icon">
            <svg-icon type="mdi" class="icon" @click="handleInput(false)" :path="cancel"></svg-icon>
            <v-btn density="compact" icon="" variant="plain" @click="handleInput(true)"><svg-icon type="mdi" class="icon check" @click="handleInput(true)" :path="check"></svg-icon></v-btn>

            <v-btn density="compact" icon="" variant="plain" @click="handleInput(false)"><svg-icon type="mdi" class="icon" :path="cancel"></svg-icon></v-btn>
        </div>
    </form>
</template>

<style scoped>
.container-input {
    display: flex;
    height: 3.5em;
}
.box-input{
    width: 80%;
    margin-right: 0.5em;
}

.container-icon {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 100%;
}
.icon {
    color: red;
    cursor: pointer;
}
.check{
    color: green;
}
</style>