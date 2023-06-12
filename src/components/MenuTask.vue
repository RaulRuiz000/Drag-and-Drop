<script setup>
import { ref } from 'vue';
import { mdiDotsVertical } from '@mdi/js'
import SvgIcon from '@jamescoyle/vue-icon'
const items = ref([
    { title: 'Editar' },
    { title: 'Eliminar' },
]);
const path = ref(mdiDotsVertical);
const emit = defineEmits(['optionMenu']);
const option = (value) => {
    emit('optionMenu', value)
};
</script>

<template>
    <v-menu transition="slide-y-transition" location="end">
        <template v-slot:activator="{ props }">
            <v-btn density="compact" icon="" variant="plain" v-bind="props"><svg-icon type="mdi" :path="path"></svg-icon></v-btn>
        </template>
        <v-list>
            <v-list-item v-for="(item, i) in items" :key="i">
                <v-list-item-title @click="option(item.title)" class="item">{{ item.title }}</v-list-item-title>
            </v-list-item>
        </v-list>
        
    </v-menu>
</template>

<style scoped>
.item{
    cursor: pointer;
}
</style>