<script lang="ts">
    import { Form, setLayoutProps } from '@inertiajs/svelte';
    import { useLang } from '@erag/lang-sync-inertia/svelte';
    import AppHead from '@/components/AppHead.svelte';
    import TextLink from '@/components/TextLink.svelte';
    import { Button } from '@/components/ui/button';
    import { Spinner } from '@/components/ui/spinner';
    import { logout } from '@/routes';
    import { send } from '@/routes/verification';

    const { __ } = useLang();

    $effect(() =>
        setLayoutProps({
            title: __('Email verification'),
            description: __(
                'Please verify your email address by clicking on the link we just emailed to you.',
            ),
        }),
    );

    let {
        status = '',
    }: {
        status?: string;
    } = $props();
</script>

<AppHead title={__('Email verification')} />

{#if status === 'verification-link-sent'}
    <div class="mb-4 text-center text-sm font-medium text-green-600">
        {__(
            'A new verification link has been sent to the email address you provided during registration.',
        )}
    </div>
{/if}

<Form {...send.form()} class="space-y-6 text-center">
    {#snippet children({ processing })}
        <Button type="submit" disabled={processing} variant="secondary">
            {#if processing}<Spinner />{/if}
            {__('Resend verification email')}
        </Button>

        <TextLink href={logout()} as="button" class="mx-auto block text-sm">
            {__('Log out')}
        </TextLink>
    {/snippet}
</Form>
