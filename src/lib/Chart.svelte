<!--<script>-->
<!--    import { onMount } from "svelte";-->
<!--    import { Chart } from "chart.js/auto";-->
<!--    import { userState } from "../state.svelte.js";-->



<!--    onMount(() => {-->
<!--        const canvas = /** @type {HTMLCanvasElement} */ (document.getElementById('canvas'));-->
<!--        const data = {-->
<!--            labels: userState.depositHistory.map(row => row.year),-->
<!--            datasets: [-->
<!--                {-->
<!--                    label: 'Vklad',-->
<!--                    data: userState.depositHistory.map(row => row.value),-->
<!--                },-->
<!--                {-->
<!--                    label: 'Úrok',-->
<!--                    data: userState.resultHistory.map(row => row.value)-->
<!--                }-->
<!--            ]-->
<!--        };-->
<!--            new Chart(-->
<!--                canvas,-->
<!--                {-->
<!--                    type: 'line',-->
<!--                    data: data,-->
<!--                    options: {-->
<!--                        responsive: true,-->
<!--                        plugins: {-->
<!--                            legend: {-->
<!--                                position: 'bottom',-->
<!--                            },-->
<!--                            title: {-->
<!--                                display: false-->
<!--                            },-->
<!--                        },-->
<!--                    },-->
<!--                }-->
<!--            );-->
<!--        });-->
<!--</script>-->

<!--<canvas id="canvas"></canvas>-->

<script>
    import { onMount } from "svelte";
    import { Chart } from "chart.js/auto";
    import { userState } from "../state.svelte.js";

    let chart;

    function createChart() {
        const canvas = /** @type {HTMLCanvasElement} */ (document.getElementById('canvas'));
        const data = {
            labels: userState.depositHistory.map(row => row.year),
            datasets: [
                {
                    label: 'Vklad',
                    data: userState.depositHistory.map(row => row.value),
                    fill: true,
                },
                {
                    label: 'Celkový zostatok',
                    data: userState.resultHistory.map(row => row.value),
                    fill: true,
                }
            ]
        };
        chart = new Chart(
            canvas,
            {
                type: 'line',
                data: data,
                options: {
                    responsive: true,
                    plugins: {
                        legend: {
                            position: 'bottom',
                        },
                        title: {
                            display: false
                        },
                    },
                },
            }
        );
    }

    onMount(() => {
        createChart();
    });

    $effect(() => {
        if (chart) {
            chart.destroy();
            createChart();
        }
    });
</script>
<div class="">
    <canvas id="canvas"></canvas>
</div>
