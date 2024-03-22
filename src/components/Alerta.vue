<template>
    <div
    class="alerta">
        <slot></slot>
    </div>
</template>


<style lang="scss" scoped>
.alerta{
    background-color: var(--blanco);
    padding: 1rem;
    font-size: 1.6rem;
    text-align: center;
    font-weight: 900;
    text-transform: uppercase;
    border-left: .5rem solid #B91C1C;
    color: #B91C1C;
    margin-bottom: 2rem;
}

</style>