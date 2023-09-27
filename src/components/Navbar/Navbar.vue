<template>
  <header>
    <nav class="navbar">
      <ul class="navbar_list">
        <slot></slot>
      </ul>
    </nav>
  </header>
</template>

<script>
  export default {
    name: 'Navbar',
  }
</script>

<style scoped>
  .navbar_list {
    display: flex;
    flex-wrap: wrap;
    column-gap: 10px;
    align-items: center;
    padding: 0;
  }
</style>