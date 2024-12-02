<script>
    import {Label, Input, ButtonGroup, Button, InputAddon} from "flowbite-svelte";
    import { CalendarMonthOutline } from "flowbite-svelte-icons";
    import { userState } from "../state.svelte.js";


    let deposit = $state(50);
    let interestRate = $state(9.0);
    let investmentLength = $state(40);

    /**
     * Kontrola na zaklade: https://www.financnykompas.sk/investovanie-pravidelne/kalkulacka#vysledok
     */
    function calculate(e) {
        e.preventDefault();
        let results = [0];
        let yearlyResults = [];
        let yearlyDeposits = [];
        let rate = (1 + interestRate/100)**(1/12) - 1;


        for(let i = 0; i < investmentLength*12; i++) {
            results.push((results[i]+deposit) * (1 + rate));
            if((i % 12 === 0 && i !== 0)) {
                yearlyResults.push({year: Math.floor(i/12), value: results[i].toFixed(2)});
                yearlyDeposits.push({year: Math.floor(i/12), value: deposit * (i)});
            }

        }
        yearlyResults.push({year: investmentLength, value: results[investmentLength*12].toFixed(2)});
        yearlyDeposits.push({year: investmentLength, value: deposit * (investmentLength*12)});

        userState.firstCalculation = true;
        userState.resultHistory = yearlyResults;
        userState.depositHistory = yearlyDeposits;

    }


</script>

<form onsubmit={calculate}>

    <div class="mb-6">
        <Label for="deposit" class="block mb-2">Mesačný vklad</Label>
        <ButtonGroup class="w-full" size="md">
            <InputAddon>€</InputAddon>
            <Input id="deposit" type="number" bind:value={deposit} required/>
        </ButtonGroup>
    </div>
    <div class="mb-6">
        <Label for="interestRate" class="block mb-2">Úroková miera</Label>
        <ButtonGroup class="w-full" size="md">
            <InputAddon>%</InputAddon>
            <Input id="interestRate" type="number" step="0.1" bind:value={interestRate} required/>
        </ButtonGroup>
    </div>
    <div class="mb-6">
        <Label for="investmentLength" class="block mb-2">Dĺžka investície v rokoch</Label>
        <ButtonGroup class="w-full" size="md">
            <InputAddon><CalendarMonthOutline/></InputAddon>
            <Input id="investmentLength" type="number" bind:value={investmentLength} required/>
        </ButtonGroup>
    </div>
    <div class="mb-6">
        <Button type="submit" color="primary">Vypočítať</Button>
    </div>
</form>