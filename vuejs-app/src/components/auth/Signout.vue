<template></template>
<script setup>
import { apiSignOut } from "@/functions/api/auth";
import { onMounted } from "vue";
import { useRouter } from "vue-router";
import { useUserStore } from "@/stores/user";
const router = useRouter();
const userStore = useUserStore();

onMounted(async () => {
  const token = userStore.getSanctumToken();
  apiSignOut(token);
  userStore.reset();
  router.replace({ name: "auth.signin" });
});
</script>