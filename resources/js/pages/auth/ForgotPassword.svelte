<script lang="ts">
    import { Form, setLayoutProps } from '@inertiajs/svelte';
    import { useLang } from '@erag/lang-sync-inertia/svelte';
    import AppHead from '@/components/AppHead.svelte';
    import InputError from '@/components/InputError.svelte';
    import TextLink from '@/components/TextLink.svelte';
    import { Button } from '@/components/ui/button';
    import { Input } from '@/components/ui/input';
    import { Label } from '@/components/ui/label';
    import { Spinner } from '@/components/ui/spinner';
    import { login } from '@/routes';
    import { email } from '@/routes/password';

    const { __ } = useLang();

    $effect(() =>
        setLayoutProps({
            title: __('Forgot password'),
            description: __(
                'Enter your email to receive a password reset link',
            ),
        }),
    );

    let {
        status = '',
    }: {
        status?: string;
    } = $props();
</script>

<AppHead title={__('Forgot password')} />

{#if status}
    <div class="mb-4 text-center text-sm font-medium text-green-600">
        {status}
    </div>
{/if}

<div class="space-y-6">
    <Form {...email.form()}>
        {#snippet children({ errors, processing })}
            <div class="grid gap-2">
                <Label for="email">{__('Email address')}</Label>
                <Input
                    id="email"
                    type="email"
                    name="email"
                    autocomplete="off"
                    placeholder="email@example.com"
                />
                <InputError message={errors.email} />
            </div>

            <div class="my-6 flex items-center justify-start">
                <Button
                    type="submit"
                    class="w-full"
                    disabled={processing}
                    data-test="email-password-reset-link-button"
                >
                    {#if processing}<Spinner />{/if}
                    {__('Email password reset link')}
                </Button>
            </div>
        {/snippet}
    </Form>

    <div class="space-x-1 text-center text-sm text-muted-foreground">
        <span>{__('Or, return to')}</span>
        <TextLink href={login()}>{__('log in')}</TextLink>
    </div>
</div>
