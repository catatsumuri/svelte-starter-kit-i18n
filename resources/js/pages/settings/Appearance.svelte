<script lang="ts">
    import { setLayoutProps } from '@inertiajs/svelte';
    import { useLang } from '@erag/lang-sync-inertia/svelte';
    import AppearanceTabs from '@/components/AppearanceTabs.svelte';
    import AppHead from '@/components/AppHead.svelte';
    import Heading from '@/components/Heading.svelte';
    import { edit as editAppearance } from '@/routes/appearance';

    const { __ } = useLang();

    setLayoutProps({
        breadcrumbs: [
            {
                title: __('Appearance settings'),
                href: editAppearance(),
            },
        ],
    });
</script>

<AppHead title={__('Appearance settings')} />

<h1 class="sr-only">{__('Appearance settings')}</h1>

<div class="space-y-6">
    <Heading
        variant="small"
        title={__('Appearance settings')}
        description={__('Update the appearance settings for your account')}
    />
    <AppearanceTabs />
</div>
