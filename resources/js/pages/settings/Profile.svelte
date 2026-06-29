<script lang="ts">
    import { Form, page, setLayoutProps } from '@inertiajs/svelte';
    import { useLang } from '@erag/lang-sync-inertia/svelte';
    import ProfileController from '@/actions/App/Http/Controllers/Settings/ProfileController';
    import AppHead from '@/components/AppHead.svelte';
    import DeleteUser from '@/components/DeleteUser.svelte';
    import Heading from '@/components/Heading.svelte';
    import InputError from '@/components/InputError.svelte';
    /* @chisel-email-verification */
    import TextLink from '@/components/TextLink.svelte';
    /* @end-chisel-email-verification */
    import { Button } from '@/components/ui/button';
    import { Input } from '@/components/ui/input';
    import { Label } from '@/components/ui/label';
    /* @chisel-email-verification */
    import { send } from '@/routes/verification';
    /* @end-chisel-email-verification */
    import { edit } from '@/routes/profile';

    const { __ } = useLang();

    $effect(() =>
        setLayoutProps({
            breadcrumbs: [
                {
                    title: __('Profile settings'),
                    href: edit(),
                },
            ],
        }),
    );

    const user = $derived(page.props.auth.user);
</script>

<AppHead title={__('Profile settings')} />

<h1 class="sr-only">{__('Profile settings')}</h1>

<div class="flex flex-col space-y-6">
    <Heading
        variant="small"
        title={__('Profile')}
        description={__('Update your name and email address')}
    />

    <Form
        {...ProfileController.update.form()}
        class="space-y-6"
        options={{ preserveScroll: true }}
    >
        {#snippet children({ errors, processing })}
            <div class="grid gap-2">
                <Label for="name">{__('Name')}</Label>
                <Input
                    id="name"
                    name="name"
                    class="mt-1 block w-full"
                    value={user.name}
                    required
                    autocomplete="name"
                    placeholder={__('Full name')}
                />
                <InputError class="mt-2" message={errors.name} />
            </div>

            <div class="grid gap-2">
                <Label for="email">{__('Email address')}</Label>
                <Input
                    id="email"
                    type="email"
                    name="email"
                    class="mt-1 block w-full"
                    value={user.email}
                    required
                    autocomplete="username"
                    placeholder={__('Email address')}
                />
                <InputError class="mt-2" message={errors.email} />
            </div>

            <!-- @chisel-email-verification -->
            {#if Boolean(page.props.mustVerifyEmail) && !user.email_verified_at}
                <div>
                    <p class="-mt-4 text-sm text-muted-foreground">
                        {__('Your email address is unverified.')}
                        <TextLink href={send()} as="button">
                            {__(
                                'Click here to re-send the verification email.',
                            )}
                        </TextLink>
                    </p>

                    {#if page.props.status === 'verification-link-sent'}
                        <div class="mt-2 text-sm font-medium text-green-600">
                            {__(
                                'A new verification link has been sent to your email address.',
                            )}
                        </div>
                    {/if}
                </div>
            {/if}
            <!-- @end-chisel-email-verification -->

            <div class="flex items-center gap-4">
                <Button
                    type="submit"
                    disabled={processing}
                    data-test="update-profile-button">{__('Save')}</Button
                >
            </div>
        {/snippet}
    </Form>
</div>

<DeleteUser />
