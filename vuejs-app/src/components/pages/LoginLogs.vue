<template>
  <div class="content-wrapper" style="min-height: 1175px;">
    <div class="content-header">
      <div class="container-fluid">
        <div class="row mb-2">
          <div class="col-sm-6">
            <h1 class="m-0">Login Activity</h1>
          </div>
          <div class="col-sm-6">
            <ol class="breadcrumb float-sm-right">
              <li class="breadcrumb-item"><a href="#">Home</a></li>
              <li class="breadcrumb-item active">Login Activity</li>
            </ol>
          </div>
        </div>
      </div>
    </div>
    <div class="content">
      <div class="container-fluid">
        <div class="card">
          <div class="card-header">
            <h3 class="card-title">Account Login History (Audit Log)</h3>
          </div>
          <div class="card-body table-responsive p-0">
            <table class="table table-hover text-nowrap">
              <thead>
                <tr>
                  <th>#</th>
                  <th>Method</th>
                  <th>Date &amp; Time</th>
                  <th>IP Address</th>
                  <th>Device / Browser</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(log, index) in logs" :key="log.id">
                  <td>{{ index + 1 }}</td>
                  <td>
                    <span class="badge" :class="log.login_method === 'Google' ? 'badge-danger' : 'badge-info'">
                      {{ log.login_method || 'Email' }}
                    </span>
                  </td>
                  <td>{{ formatDate(log.logged_in_at) }}</td>
                  <td>{{ log.ip_address }}</td>
                  <td>{{ log.user_agent }}</td>
                </tr>
                <tr v-if="logs.length === 0">
                  <td colspan="5" class="text-center">No login activity yet.</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref, onMounted } from "vue";
import { apiGetLoginLogs } from "@/functions/api/auth";

const logs = ref([]);

function formatDate(value) {
  return new Date(value).toLocaleString();
}

onMounted(async () => {
  try {
    const response = await apiGetLoginLogs();
    logs.value = response.data.logs;
  } catch (error) {
    console.error(error);
  }
});
</script>