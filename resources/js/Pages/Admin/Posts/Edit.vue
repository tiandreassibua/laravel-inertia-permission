<script setup>
import AdminLayout from "@/Layouts/AdminLayout.vue";
import { Head, Link, useForm } from "@inertiajs/vue3";

import InputError from "@/Components/InputError.vue";
import InputLabel from "@/Components/InputLabel.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import TextInput from "@/Components/TextInput.vue";

const props = defineProps({
    post: {
        type: Object,
        required: true,
    }
});

const form = useForm({
    title: props.post?.title
});
</script>

<template>
    <Head title="Update role" />

    <AdminLayout>
        <div class="max-w-7xl mx-auto py-4">
            <div class="flex justify-between items-center">
                <Link :href="route('posts.index')"
                    class="px-3 py-2 text-white font-semibold bg-indigo-500 hover:bg-indigo-700 rounded">Back</Link>
            </div>
            <div class="mt-6 max-w-6xl mx-auto bg-slate-100 p-6 rounded-md shadow-md">
                <h1 class="text-xl font-semibold text-indigo-700">
                    Update Post
                </h1>
                <form @submit.prevent="
                    form.put(route('posts.update', props.post.id))
                    ">
                    <div class="mt-4">
                        <InputLabel for="title" value="Title" />

                        <TextInput id="title" type="text" class="mt-1 block w-full" v-model="form.title"
                            autocomplete="title" />

                        <InputError class="mt-2" :message="form.errors.title" />
                    </div>

                    <div class="flex items-center mt-4">
                        <PrimaryButton :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
                            Update
                        </PrimaryButton>
                    </div>
                </form>
            </div>
        </div>
    </AdminLayout>
</template>
