<script setup>
import { ref, computed} from 'vue';
import Alerta from './Alerta.vue';
import cerrarModal from '../assets/img/cerrar.svg'

const error = ref('')

const emit = defineEmits(['ocultar-modal','guardar-gasto','eliminar-gasto','update:nombre','update:cantidad','update:categoria'])
const props = defineProps({
    modal: {
        type: Object,
        required: true
    },
    nombre: {
        type: String,
        required: true
    },
    cantidad: {
        type: [String, Number],
        required: true
    },
    categoria: {
        type: String,
        required: true
    },
    disponible: {
        type: Number,
        required: true
    },
    id: {
        type: [String, null],
        required: false
    }
})

const old = props.cantidad

const agregarGasto = (e) =>{
    //validar campos vacios
     const {nombre, cantidad, categoria, disponible, id} = props
     if([nombre, cantidad, categoria].includes('')){
         error.value = 'Todos los campos son obligatorios'
         setTimeout(() => {
             error.value = ''
         }, 3000);
         return
     }
    //validar cantidad
    if(cantidad <= 0 || isNaN(cantidad)){
        error.value = 'Cantidad no válida'
        setTimeout(() => {
            error.value = ''
        }, 3000);
        return
    }
    //validar que el usuario no gaste mas de lo disponible
    if(id){
        if(cantidad > disponible + old){
            error.value = 'Has exedido el presupuesto disponible'
            setTimeout(() => {
                error.value = ''
            }, 3000);
            return
        }
    }else{
        if(cantidad > disponible){
            error.value = 'Has exedido el presupuesto disponible'
            setTimeout(() => {
                error.value = ''
            }, 3000);
            return
        }
    }
    //guardar gasto
    emit('guardar-gasto')
}

const isEditing = computed(() => {return props.id})

</script>

<template>
    <div class="modal">
        <div class="cerrar-modal">
            <img 
                :src="cerrarModal"
                @click="$emit('ocultar-modal')"
            />
        </div>
        <div 
            class="contenedor contenedor-formulario"
            :class="[modal.animar ? 'animar' : 'cerrar']"
        >
            <form
                class="nuevo-gasto"
                @submit.prevent="agregarGasto"
            >
                <legend>{{isEditing ? 'Editar Gasto' : 'Añadir Gasto'}}</legend>
                <Alerta
                    v-if="error">
                    {{ error }}
                </Alerta>
                <div class="campo">
                    <label for="nombre">Nombre Gasto:</label>
                    <input 
                        type="text" 
                        id="nombre" 
                        placeholder="Añade el Nombre del Gasto" 
                        :value="nombre"
                        @input="$emit('update:nombre', $event.target.value)"
                    />
                </div>
                <div class="campo">
                    <label for="cantidad">Cantidad:</label>
                    <input 
                        type="text" 
                        id="cantidad" 
                        placeholder="Añade la cantidad del Gasto, ej: 300"
                        :value="cantidad"
                        @input="$emit('update:cantidad', +$event.target.value)"
                    />
                </div>
                <div class="campo">
                    <label for="categoria">Categoria:</label>
                    <select 
                        id="categoria"
                        :value="categoria"
                        @input="$emit('update:categoria', $event.target.value)"
                    >
                        <option value="">-- Seleccione --</option>
                        <option value="ahorro">Ahorro</option>
                        <option value="comida">Comida</option>
                        <option value="casa">Casa</option>
                        <option value="gastos">Gastos Varios</option>
                        <option value="ocio">Ocio</option>
                        <option value="salud">Salud</option>
                        <option value="suscripciones">Suscripciones</option>
                    </select>
                </div>
                <input 
                    type="submit"
                    :value="[isEditing ? 'Guardar Cambios' : 'Añadir Gasto']"
                />
            </form>
            <button 
                v-if="isEditing"
                type="button"
                class="btn-eliminar"
                @click="$emit('eliminar-gasto', props.id)"
            >
                Eliminar Gasto
            </button>
        </div>
    </div>
</template>

<style scoped>
.modal{
    background-color: rgba(0,0,0,.9);
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
}	
.cerrar-modal{
    position: absolute;
    top: 3rem;
    right: 3rem;
    cursor: pointer;
}
.contenedor-formulario{
    transition: all 300ms ease-in;
    opacity: 0;
}
.contenedor-formulario.animar{
    opacity: 1;
}
.contenedor-formulario.cerrar{
    opacity: 0;
}
.cerrar-modal img{
    width: 3rem;
}
.nuevo-gasto{
    padding: 3rem;
    border-radius: 1.2rem;
    width: 100%;
    display: grid;
    gap: 2rem;
}
.nuevo-gasto legend{
    font-size: 3rem;
    color: var(--blanco);
    text-align: center;
    font-weight: 700;
}
.nuevo-gasto input, .nuevo-gasto select{
    width: 100%;
    padding: 1rem;
    margin: 1rem 0;
    border: none;
    border-radius: .8rem;
    background-color: var(--gris-claro);
    font-size: 2.2rem;
}
.nuevo-gasto input[type="submit"]{
    background-color: var(--azul);
    color: var(--blanco);
    font-size: 2.2rem;
    font-weight: j00;
    text-transform: uppercase;
    cursor: pointer;
    transition-property: background-color;
    transition-duration: 300ms;
}
.nuevo-gasto label{
    font-size: 3rem;
    color: var(--blanco);
}
.campo{
    display: grid;
    gap:2rem;
}
.btn-eliminar{
   border: none;
   padding: 1rem;
   width: 100%;
   background-color: #ef4444;
   font-size: 1.2rem;
   color: var(--blanco);
   margin-top: 10rem;
   cursor: pointer;
}
</style>