<template>
    <div class="bg-black">
        <div class="card flex justify-center">
            <Form :resolver="resolver" @submit="onFormSubmit" class="flex flex-col gap-4 w-full sm:w-56">
                <FormField v-slot="$field" as="section" name="username" initialValue="" class="flex flex-col gap-2">
                    <InputText type="text" placeholder="Username" />
                    <Message v-if="$field?.invalid" severity="error" size="small" variant="simple">{{ $field.error?.message }}</Message>
                </FormField>
                <FormField v-slot="$field" asChild name="password" initialValue="">
                    <section class="flex flex-col gap-2">
                        <Password type="text" placeholder="Password" :feedback="false" toggleMask fluid />
                        <Message v-if="$field?.invalid" severity="error" size="small" variant="simple">{{ $field.error?.message }}</Message>
                    </section>
                </FormField>
                <Button type="submit" severity="secondary" label="Submit" />
            </Form>
        </div>
    </div>
</template>

<script lang="ts" setup>
import { zodResolver } from '@primevue/forms/resolvers/zod';
import { z } from 'zod';
import { useToast } from 'primevue/usetoast';

const toast = useToast();

const resolver = zodResolver(
    z.object({
        username: z.string().min(1, { message: 'Username is required.' }),
        password: z.string().min(1, { message: 'Password is required.' })
    })
);

const onFormSubmit = ({ valid }: { valid: boolean }) => {
    if (valid) {
        toast.add({ severity: 'success', summary: 'Form is submitted.', life: 3000 });
    }
};
</script>