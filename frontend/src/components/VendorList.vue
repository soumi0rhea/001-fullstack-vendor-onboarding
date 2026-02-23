<template>
  <div class="vendor-list">
    <h2>Vendor List</h2>
    <div v-if="vendorStore.loading">Loading vendors...</div>
    <div v-else-if="vendorStore.vendors.length === 0" class="no-vendors" role="status" aria-live="polite">
      <p class="no-vendors-title">No vendors found.</p>
      <p class="no-vendors-desc">Add your first vendor using the form.</p>
    </div>
    <table v-else class="vendors-table" role="table" aria-label="Vendor list">
      <thead>
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Contact Person</th>
          <th>Email</th>
          <th>Partner Type</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="vendor in vendorStore.vendors"
          :key="vendor.id"
          tabindex="0"
          role="row"
          :aria-label="`Vendor ${vendor.name}`"
        >
          <td role="cell">{{ vendor.id }}</td>
          <td role="cell">{{ vendor.name }}</td>
          <td role="cell">{{ vendor.contact_person }}</td>
          <td role="cell">{{ vendor.email }}</td>
          <td role="cell">{{ vendor.partner_type }}</td>
          <td role="cell"><button @click="deleteVendor(vendor)" class="delete-btn">Delete</button></td>
        </tr>
      </tbody>
    </table>
    <div v-if="success" class="success-message">Vendor deleted successfully!</div>
    <div v-if="vendorStore.error" class="error">{{ vendorStore.error }}</div>
  </div>
  <dialog ref="dialogRef" class="delete-dialog" aria-modal="true" role="dialog">
    <h2 class="delete-dialog-title">Delete Vendor</h2>
    <p class="delete-dialog-content">This action cannot be undone. Are you sure you want to delete <strong>{{ selectedVendor?.name }}</strong>?</p>
    <button class="cancel-btn" @click="closeDialog">Cancel</button>
    <button class="delete-confirm-btn" @click="confirmDelete">Delete</button>
  </dialog>
</template>


<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useVendorStore } from '../stores/vendorStore';
import type { Vendor } from '../types/Vendor';

// Using the vendor store directly, no need for local props or state
const vendorStore = useVendorStore();
const success = ref(false);
const selectedVendor = ref<Vendor | null>(null);

const dialogRef = ref(null);

onMounted(() => {
  vendorStore.fetchVendors();
});

const deleteVendor = (vendor) => {
  openDialog(vendor);
};  

const confirmDelete = async () => {
  if(vendorStore.loading) return;
  try {
    await vendorStore.deleteVendor(selectedVendor.value?.id);
    success.value = true;
    setTimeout(() => {
      success.value = false;
    }, 2000);
  } catch (err) {
    // Error is already handled in the store
  } finally {
    closeDialog();
  }
}

const openDialog = (vendor) => {
  selectedVendor.value = vendor;
  if (dialogRef.value) {
    dialogRef.value.showModal();
  }
}

const closeDialog = () => {
  if (dialogRef.value) {
    dialogRef.value.close();
  }
  selectedVendor.value = null;
}

</script>

<style scoped>
.vendor-list {
  margin: 20px 0;
}

.vendors-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 15px;
}

.vendors-table th,
.vendors-table td {
  padding: 10px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.vendors-table th {
  background-color: #f2f2f2;
  font-weight: bold;
}

.vendors-table tbody tr {
  transition: background-color 120ms ease, box-shadow 120ms ease;
}

.vendors-table tbody tr:nth-child(even) {
  background-color: #fbfbfb;
}

.vendors-table tbody tr:hover,
.vendors-table tbody tr:focus-visible {
  background-color: #e8f0ff;
  box-shadow: inset 3px 0 0 0 rgba(3, 102, 214, 0.12);
  outline: none;
  cursor: pointer;
}

.vendors-table tbody tr:focus-visible {
  box-shadow: inset 3px 0 0 0 rgba(3, 102, 214, 0.2), 0 0 0 3px rgba(3, 102, 214, 0.12);
}

.error {
  color: red;
  padding: 10px;
}

.no-vendors {
  padding: 20px;
  text-align: center;
  color: #444;
  background: #fafafa;
  border: 1px dashed #e6e6e6;
  border-radius: 6px;
}

.no-vendors-title {
  margin: 0 0 6px 0;
  font-weight: 600;
}

.no-vendors-desc {
  margin: 0;
  color: #666;
}

.success-message {
  color: #4CAF50;
  margin-top: 10px;
}

.delete-dialog {
  border: none;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.3);
  margin: auto auto;
}

.delete-dialog::backdrop {
  backdrop-filter: blur(5px);
  background-color: rgba(0, 0, 0, 0.4);
}

.delete-dialog-title {
  margin-top: 0;
}
.delete-dialog-content {
  margin: 20px 0;
}
.cancel-btn {
  padding: 10px 15px;
  background-color: #ccc;
  color: #333;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  margin-right: 10px;
}
.cancel-btn:hover {
  background-color: #bbb;
}
</style>