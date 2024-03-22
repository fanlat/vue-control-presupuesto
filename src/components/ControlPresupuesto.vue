<script setup> 
import { computed } from 'vue';
import CircleProgress from 'vue3-circle-progress'
import "vue3-circle-progress/dist/circle-progress.css"
import { formatearCantidad } from '../helpers';


defineEmits(['reset-app','mostar-modal-presupuesto'])

const props = defineProps({
    presupuesto: {
        type: Number,
        required: true
    },
    disponible: {
        type: Number,
        required: true
    },
    gastado: {
        type: Number,
        required: true
    }
})

const porcentaje = computed(() => {
    return parseInt((  props.gastado * 100 ) / props.presupuesto )
})
</script>

<template>
    <div class="dos-columnas">
        <div class="contenedor-grafico">
            <p class="porcentaje">{{porcentaje}}%</p>
            <CircleProgress
                :percent="porcentaje"
                :size="250"
                :border-width="20"
                :border-bg-width="20"
                fill-color="#3b82f6"
                empty-color="#f5f5f5"
            />
        </div>
        <div class="contenedor-presupuesto">
            <button
                class="reset-app"
                type="button"
                @click="$emit('reset-app')"
            >Resetear App</button>
            <p
                class="presupuesto"
                @click="$emit('mostar-modal-presupuesto')"
            >
                <span>Presupuesto:</span>
                {{ formatearCantidad(presupuesto) }}
                <svg  xmlns="http://www.w3.org/2000/svg"  width="24"  height="24"  viewBox="0 0 24 24"  fill="none"  stroke="currentColor"  stroke-width="2"  stroke-linecap="round"  stroke-linejoin="round"  class="icon icon-tabler icons-tabler-outline icon-tabler-pencil"><path stroke="none" d="M0 0h24v24H0z" fill="none"/><path d="M4 20h4l10.5 -10.5a2.828 2.828 0 1 0 -4 -4l-10.5 10.5v4" /><path d="M13.5 6.5l4 4" /></svg>
            </p>
            <p>
                <span>Disponible:</span>
                {{ formatearCantidad(disponible) }}
            </p>
            <p>
                <span>Gastado:</span>
                {{ formatearCantidad(gastado) }}
            </p>
        </div>
    </div>
</template>

<style scoped>
.contenedor-grafico{
    position: relative;
}
.porcentaje{
    position: absolute;
    margin: auto;
    top:calc(50% - 1.5rem);
    left: 0;
    right: 0;
    text-align: center;
    z-index: 2;
    font-size: 3rem;
    font-weight: 900;
    color: var(--gris-oscuro);
}
    .dos-columnas{
        display: flex;
        flex-direction: column;
    }
    .dos-columnas > :first-child{
        margin-bottom: 3rem;
    }
    .contenedor-presupuesto{
        width: 100%;
    }
    .contenedor-presupuesto p{
        font-size: 2.4rem;
        text-align: center;
        color: var(--gris-oscuro);
    }
    .contenedor-presupuesto span{
        font-weight: 900;
        color: var(--azul);
    }
    .reset-app{
        background-color: #db2777;
        border: none;
        padding: 1rem;
        width: 100%;
        cursor: pointer;
        color: var(--blanco);
        font-weight: 900;
        text-transform: uppercase;
        border-radius: 1rem;
        transition-property: background-color;
        transition-duration: 300ms;
    }
    .reset-app:hover{
        background-color: #c11d67;
    }
    .presupuesto{
        cursor: pointer;
    }
    @media (width > 768px){
        .dos-columnas{
            flex-direction: row;
            gap:4rem;
            align-items: center;
        }
        .dos-columnas > :first-child{
            margin-bottom: 0;
        }
        .contenedor-presupuesto p{
            text-align: left;
        }
    }
</style>