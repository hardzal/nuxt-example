<template>
  <div>
    <h1>Hi, I'm guest!</h1>
  </div>
</template>

<script setup lang="ts">
definePageMeta({
  auth: {
    unauthenticatedOnly: true,
    navigateAuthenticatedTo: "/protected",
  },
});
</script>
