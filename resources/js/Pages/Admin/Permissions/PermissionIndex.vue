<script setup>
import AdminLayout from "@/Layouts/AdminLayout.vue";
import { Head, Link } from "@inertiajs/vue3";

import Table from "@/Components/Table.vue";
import TableRow from "@/Components/TableRow.vue";
import TableHeaderCell from "@/Components/TableHeaderCell.vue";
import TableDataCell from "@/Components/TableDataCell.vue";

defineProps(["permissions"]);
</script>

<template>
    <Head title="Permissions" />

    <AdminLayout>
        <div class="max-w-7xl mx-auto py-4">
            <div class="flex justify-between items-center">
                <h1 class="font-semibold text-xl">Permissions Index Page</h1>
                <Link
                    :href="route('permissions.create')"
                    class="px-3 py-2 text-white font-semibold bg-indigo-500 hover:bg-indigo-700 rounded"
                    >New Permission</Link
                >
            </div>
            <div class="mt-6">
                <Table>
                    <template #header>
                        <TableRow>
                            <TableHeaderCell>ID</TableHeaderCell>
                            <TableHeaderCell>Name</TableHeaderCell>
                            <TableHeaderCell>Action</TableHeaderCell>
                        </TableRow>
                    </template>
                    <template #default>
                        <TableRow
                            v-if="permissions.length > 0"
                            v-for="permission in permissions"
                            :key="permission.id"
                            class="border-b"
                        >
                            <TableDataCell>{{ permission.id }}</TableDataCell>
                            <TableDataCell>{{ permission.name }}</TableDataCell>
                            <TableDataCell class="space-x-2">
                                <Link
                                    :href="route('permissions.edit', permission.id)"
                                    class="text-green-500 font-semibold hover:text-green-600"
                                    >Edit</Link
                                >
                                <Link
                                    :href="route('permissions.destroy', permission.id)"
                                    method="delete"
                                    as="button"
                                    class="text-red-500 font-semibold hover:text-red-600"
                                    >Delete</Link
                                >
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
