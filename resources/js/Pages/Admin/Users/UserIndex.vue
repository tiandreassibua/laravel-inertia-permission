<script setup>
import AdminLayout from "@/Layouts/AdminLayout.vue";
import { Head, Link, useForm } from "@inertiajs/vue3";

import Modal from "@/Components/Modal.vue";
import DangerButton from "@/Components/DangerButton.vue";
import SecondaryButton from "@/Components/SecondaryButton.vue";
import Table from "@/Components/Table.vue";
import TableRow from "@/Components/TableRow.vue";
import TableHeaderCell from "@/Components/TableHeaderCell.vue";
import TableDataCell from "@/Components/TableDataCell.vue";
import { ref } from "vue";

defineProps(["users"]);
const form = useForm({});

const deleteUser = (userId) => {
    form.delete(route("users.destroy", userId), {
        onSuccess: () => closeModal(),
    });
};

const showConfirmDeleteUserModel = ref(false);

const closeModal = () => {
    showConfirmDeleteUserModel.value = false;
};
const confirmDeleteUser = () => {
    showConfirmDeleteUserModel.value = true;
};
</script>

<template>
    <Head title="Users" />

    <AdminLayout>
        <div class="max-w-7xl mx-auto py-4">
            <div class="flex justify-between items-center">
                <h1 class="font-semibold text-xl">Users Index Page</h1>
                <Link
                    :href="route('users.create')"
                    class="px-3 py-2 text-white font-semibold bg-indigo-500 hover:bg-indigo-700 rounded"
                    >New User</Link
                >
            </div>
            <div class="mt-6">
                <Table>
                    <template #header>
                        <TableRow>
                            <TableHeaderCell>ID</TableHeaderCell>
                            <TableHeaderCell>Name</TableHeaderCell>
                            <TableHeaderCell>Email</TableHeaderCell>
                            <TableHeaderCell>Action</TableHeaderCell>
                        </TableRow>
                    </template>
                    <template #default>
                        <TableRow
                            v-if="users.length > 0"
                            v-for="user in users"
                            :key="user.id"
                            class="border-b"
                        >
                            <TableDataCell>{{ user.id }}</TableDataCell>
                            <TableDataCell>{{ user.name }}</TableDataCell>
                            <TableDataCell>{{ user.email }}</TableDataCell>
                            <TableDataCell class="space-x-2">
                                <Link
                                    :href="route('users.edit', user.id)"
                                    class="text-green-500 font-semibold hover:text-green-600"
                                    >Edit</Link
                                >
                                <!-- <Link
                                    :href="route('users.destroy', user.id)"
                                    method="delete"
                                    as="button"
                                    class="text-red-500 font-semibold hover:text-red-600"
                                    >Delete</Link
                                > -->
                                <button
                                    @click="confirmDeleteUser"
                                    class="text-red-400 hover:text-red-600"
                                >
                                    Delete
                                </button>
                                <Modal
                                    :show="showConfirmDeleteUserModel"
                                    @close="closeModal"
                                >
                                    <div class="p-6">
                                        <h2
                                            class="text-lg font-semibold text-slate-800"
                                        >
                                            Are you sure to delete this user?
                                        </h2>
                                        <div
                                            class="mt-6 flex justify-end space-x-4"
                                        >
                                            <DangerButton
                                                @click="deleteUser(user.id)"
                                                >Delete</DangerButton
                                            >
                                            <SecondaryButton @click="closeModal"
                                                >Cancel</SecondaryButton
                                            >
                                        </div>
                                    </div>
                                </Modal>
                            </TableDataCell>
                        </TableRow>
                        <TableRow v-else>
                            <TableDataCell colspan="3" class="text-center"
                                >No data found.</TableDataCell
                            >
                        </TableRow>
                    </template>
                </Table>
            </div>
        </div>
    </AdminLayout>
</template>
