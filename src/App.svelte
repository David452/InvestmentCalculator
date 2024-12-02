<script>
    import "./app.css";
    import Form from "./lib/Form.svelte";
    import Result from "./lib/Result.svelte";
    import Chart from "./lib/Chart.svelte";
    import { userState } from "./state.svelte.js";
    import { fade, fly } from "svelte/transition";

    $effect(() => {
        userState.cols = userState.firstCalculation ? 2 : 1;
    });

</script>

<div class="grid grid-rows-{userState.cols} sm:grid-cols-{userState.cols} justify-items-center content-center min-h-svh py-16">

    <div id="form" class="w-full sm:w-auto  self-center px-16 mb-16" transition:fly={{ x: !userState.firstCalculation ? -200 : 0, duration: 500 }}>
        <Form />
    </div>

    {#if userState.firstCalculation}
        <div class="justify-self-start w-full pl-2" transition:fade={{duration: 1000, delay: 500}}>
            <Result text="Suma na konci obdobia:" number={userState.resultHistory.slice(-1)[0].value} />
            <Result text="Z toho vklad:" number={userState.depositHistory.slice(-1)[0].value} />
            <Result text="Z toho úrok:" number={parseFloat(userState.resultHistory.slice(-1)[0].value) - parseFloat(userState.depositHistory.slice(-1)[0].value)} />
            <Chart />
        </div>
    {/if}

</div>

<!--<style>-->
<!--    @media (min-width: 640px) {-->
<!--        #form {-->
<!--            width: auto;-->
<!--        }-->
<!--    }-->
<!--</style>-->