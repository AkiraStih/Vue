<script setup>

</script>

<template>
  <button class="button">
    <slot/>
    </button>
</template>

<style scoped>
  .button{
    border: none;
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 10px;
    border-radius: 10px;
    background: var(--gradient);
    padding: 14px;
    font-family: var(--font);
    font-size: 20px;
    font-weight: 600;
    color: var(--color-primary);
    cursor: pointer;
    transition: all 0.5s ease;
    height: 53px;
    width: 100%;
  }
  .button:hover{
    filter: hue-rotate(-50deg);
  }
</style>