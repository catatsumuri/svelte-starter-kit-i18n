<script lang="ts">
    import { router, page } from '@inertiajs/svelte';
    import {
        Select,
        SelectContent,
        SelectItem,
        SelectTrigger,
    } from '@/components/ui/select';
    import { switchMethod } from '@/routes/locale';

    const locale = $derived((page.props.locale as string) ?? '');
    const locales = $derived(
        (page.props.locales as Record<string, string>) ?? {},
    );

    function handleChange(value: string) {
        router.post(
            switchMethod.url(),
            { locale: value },
            { preserveScroll: true },
        );
    }
</script>

<Select value={locale} onValueChange={handleChange}>
    <SelectTrigger class="w-auto gap-2 text-xs">
        {locales[locale] ?? locale}
    </SelectTrigger>
    <SelectContent>
        {#each Object.entries(locales) as [code, name] (code)}
            <SelectItem value={code} class="text-xs">{name}</SelectItem>
        {/each}
    </SelectContent>
</Select>
