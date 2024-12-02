<script>
    import { Heading } from "flowbite-svelte";

    let { text, number } = $props();

    function formatNumber(num) {
        return new Intl.NumberFormat('sk-SK').format(num);
    }
</script>
<div class="mb-4">
    <Heading tag="h2" class="">{text}</Heading>
    <Heading tag="h3" class="mb-2 text-emerald-600 hover:text-emerald-700 hover:underline">{formatNumber(number)} €</Heading>
</div>
