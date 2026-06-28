<script lang="ts">
    import { Form, setLayoutProps } from '@inertiajs/svelte';
    import { useLang } from '@erag/lang-sync-inertia/svelte';
    import AppHead from '@/components/AppHead.svelte';
    import InputError from '@/components/InputError.svelte';
    import PasswordInput from '@/components/PasswordInput.svelte';
    import TextLink from '@/components/TextLink.svelte';
    import { Button } from '@/components/ui/button';
    import { Input } from '@/components/ui/input';
    import { Label } from '@/components/ui/label';
    import { Spinner } from '@/components/ui/spinner';
    import { login } from '@/routes';
    import { store } from '@/routes/register';

    const { __ } = useLang();

    setLayoutProps({
        title: __('Create an account'),
        description: __('Enter your details below to create your account'),
    });

    let { passwordRules }: { passwordRules: string } = $props();
</script>

<AppHead title={__('Register')} />

<Form
    {...store.form()}
    resetOnSuccess={['password', 'password_confirmation']}
    class="flex flex-col gap-6"
>
    {#snippet children({ errors, processing })}
        <div class="grid gap-6">
            <div class="grid gap-2">
                <Label for="name">{__('Name')}</Label>
                <Input
                    id="name"
                    type="text"
                    required
                    autocomplete="name"
                    name="name"
                    placeholder={__('Full name')}
                />
                <InputError message={errors.name} />
            </div>

            <div class="grid gap-2">
                <Label for="email">{__('Email address')}</Label>
                <Input
                    id="email"
                    type="email"
                    required
                    autocomplete="email"
                    name="email"
                    placeholder="email@example.com"
                />
                <InputError message={errors.email} />
            </div>

            <div class="grid gap-2">
                <Label for="password">{__('Password')}</Label>
                <PasswordInput
                    id="password"
                    required
                    autocomplete="new-password"
                    name="password"
                    placeholder={__('Password')}
                    passwordrules={passwordRules}
                />
                <InputError message={errors.password} />
            </div>

            <div class="grid gap-2">
                <Label for="password_confirmation"
                    >{__('Confirm password')}</Label
                >
                <PasswordInput
                    id="password_confirmation"
                    required
                    autocomplete="new-password"
                    name="password_confirmation"
                    placeholder={__('Confirm password')}
                    passwordrules={passwordRules}
                />
                <InputError message={errors.password_confirmation} />
            </div>

            <Button
                type="submit"
                class="mt-2 w-full"
                disabled={processing}
                data-test="register-user-button"
            >
                {#if processing}<Spinner />{/if}
                {__('Create account')}
            </Button>
        </div>

        <div class="text-center text-sm text-muted-foreground">
            {__('Already have an account?')}
            <TextLink href={login()} class="underline underline-offset-4">
                {__('Log in')}
            </TextLink>
        </div>
    {/snippet}
</Form>
