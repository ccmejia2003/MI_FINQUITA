<script setup>
import { ref } from 'vue'
import VueTailwindDatepicker from "vue-tailwind-datepicker";
import { useSalesStore } from '@/stores/sales'
import SalesDetails from '@/components/SalesDetails.vue';
import { formatCurrency } from '@/helpers';
const sales = useSalesStore()
const formater = ref({
    date: 'DD/MM/YYYY',
    month: 'MMMM'
})

</script>
<template>
    <h1 class="text-4xl font-black my-4">Resumen de Ventas</h1>
    <div class="md:flex md:items-start gap-5">
        <div class="md:w-1/2 lg:w-1/3 bg-white flex justify-center p-5">
            <vue-tailwind-datepicker v-model="sales.date" i18n="es-mx" as-single="true" no-input :formatter="formater" />
        </div>
        <div class="md:w-1/2 lg:w-2/3  space-y-5 lg:h-screen lg:overflow-y-scroll p-5 pb-32">
            <p class="text-center text-lg" v-if="sales.isDateSelect">Ventas de la fecha: <span class="font-black">{{
                sales.date }}</span></p>
            <p v-else class="text-center text-lg font-extrabold">Selecciona una Fecha</p>

            <div v-if="sales.salesCollection.length" class="space-y-5">
                <p class="text-right text-2xl ">Total del día:
                    <span class="font-black">
                        {{ formatCurrency(sales.totalSalesOfDay) }}
                    </span>
                </p>
                <SalesDetails v-for="sale in sales.salesCollection" :key="sale.id" :sale="sale" />

            </div>
            <p v-else-if="sales.noSales" class="text-lg text-center">No hay Ventas en
                este día</p>
        </div>

    </div>
</template>

