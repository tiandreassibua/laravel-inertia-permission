<script setup>
import AdminLayout from "@/Layouts/AdminLayout.vue";
import { Head, Link, useForm } from "@inertiajs/vue3";

import InputError from "@/Components/InputError.vue";
import InputLabel from "@/Components/InputLabel.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import TextInput from "@/Components/TextInput.vue";

import VueMultiselect from "vue-multiselect";

defineProps({
    permissions: Array,
});

const form = useForm({
    name: "",
    permissions: [],
});
</script>

<template>
    <Head title="Create new Role" />

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
                <h1 class="text-xl font-semibold text-indigo-700 py-2">Create New Role</h1>
                <form @submit.prevent="form.post(route('roles.store'))">
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
                            Create
                        </PrimaryButton>
                    </div>
                </form>
            </div>
        </div>
    </AdminLayout>
</template>
<style src="vue-multiselect/dist/vue-multiselect.css"></style>
