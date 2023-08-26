<script setup>
import AdminLayout from "@/Layouts/AdminLayout.vue";
import { Head, Link, useForm } from "@inertiajs/vue3";

import InputError from "@/Components/InputError.vue";
import InputLabel from "@/Components/InputLabel.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import TextInput from "@/Components/TextInput.vue";
import VueMultiselect from "vue-multiselect";

import Table from "@/Components/Table.vue";
import TableRow from "@/Components/TableRow.vue";
import TableHeaderCell from "@/Components/TableHeaderCell.vue";
import TableDataCell from "@/Components/TableDataCell.vue";
import { onMounted } from "vue";
import { watch } from "vue";

const props = defineProps({
    role: {
        type: Object,
        required: true,
    },
    permissions: Array,
});

const form = useForm({
    name: props.role.name,
    permissions: [],
});

onMounted(() => {
    form.permissions = props.role?.permissions;
});

watch(
    () => props.role,
    () => (form.permissions = props.role?.permissions)
);
</script>

<template>
    <Head title="Update role" />

    <AdminLayout>
        <div class="max-w-7xl mx-auto py-4">
            <div class="flex justify-between items-center">
                <Link
                    :href="route('roles.index')"
                    class="px-3 py-2 text-white font-semibold bg-indigo-500 hover:bg-indigo-700 rounded"
                    >Back</Link
                >
            </div>
            <div
                class="mt-6 max-w-6xl mx-auto bg-slate-100 p-6 rounded-md shadow-md"
            >
                <h1 class="text-xl font-semibold text-indigo-700">
                    Update Role
                </h1>
                <form
                    @submit.prevent="
                        form.put(route('roles.update', props.role.id))
                    "
                >
                    <div class="mt-4">
                        <InputLabel for="name" value="Name" />

                        <TextInput
                            id="name"
                            type="text"
                            class="mt-1 block w-full"
                            v-model="form.name"
                            autocomplete="name"
                        />

                        <InputError class="mt-2" :message="form.errors.name" />
                    </div>

                    <div class="mt-4">
                        <InputLabel for="permissions" value="Permissions" />
                        <VueMultiselect
                            id="permissions"
                            v-model="form.permissions"
                            :options="permissions"
                            :multiple="true"
                            :close-on-select="false"
                            placeholder="Pick some"
                            label="name"
                            track-by="id"
                        />
                    </div>

                    <div class="flex items-center mt-4">
                        <PrimaryButton
                            :class="{ 'opacity-25': form.processing }"
                            :disabled="form.processing"
                        >
                            Update
                        </PrimaryButton>
                    </div>
                </form>
            </div>
            <div
                class="mt-6 max-w-6xl mx-auto bg-slate-100 p-6 rounded-md shadow-md"
            >
                <h1 class="text-xl font-semibold text-indigo-700">
                    Permissions
                </h1>
                <div class="bg-white">
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
                                v-if="role.permissions != 0"
                                v-for="rolePermission in role.permissions"
                                :key="rolePermission.id"
                                class="border-b"
                            >
                                <TableDataCell>{{
                                    rolePermission.id
                                }}</TableDataCell>
                                <TableDataCell>{{
                                    rolePermission.name
                                }}</TableDataCell>
                                <TableDataCell class="space-x-2">
                                    <Link
                                        :href="
                                            route('roles.permissions.destroy', [
                                                role.id,
                                                rolePermission.id,
                                            ])
                                        "
                                        method="DELETE"
                                        as="button"
                                        class="text-red-500 font-semibold hover:text-red-600"
                                        >Revoke</Link
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
        </div>
    </AdminLayout>
</template>
<style src="vue-multiselect/dist/vue-multiselect.css"></style>
