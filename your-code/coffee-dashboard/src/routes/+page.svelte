<script lang="ts">
	import { onMount } from "svelte";

    let boroughData: {borough: string, jobs: number, hourlyEarnings:number}[] = [];
    let coffeeShopData: {shop: string, borough: string, revenue: number}[] = [];
    
    let filteredCoffeeShopList: {shop: string, borough: string, revenue: number}[] = [];

    let selectedBorough: string = '';
    let totalRevenue: number = 0;
    let hourlyEarnings: number = 0;

    // fetch and set borough data
    async function getBoroughData(): Promise<void>{
        
        // fetch data
        const response = await fetch('http://localhost:3000/allboroughs');
        if (!response.ok){
            // error handling here
            alert('error fetching borough data')
        }
        const data = await response.json();

        // set borough data 
        boroughData = data.borough.map((borough: { borough: string; jobs: string; hourlyearnings: string; }) => {
            return {
                borough: borough.borough,
                jobs: Number(borough.jobs),
                hourlyEarnings: Number(borough.hourlyearnings)
            }
        });    
    }
    
    // fetch and set coffee shop data
    async function getCoffeeShopData(): Promise<void>{
        
        // fetch data
        const response = await fetch('http://localhost:3000/coffeeshops');
        if (!response.ok){
            // error handling here
            alert('error fetching coffee data')
        }
        const data = await response.json();

        // set coffee shop data
        coffeeShopData = data.coffeeshops.map((shop: { shop: string; borough: string; revenue: number; }) => {
            return {
                shop: shop.shop,
                borough: shop.borough,
                revenue: shop.revenue
            }
        });
    }

    // on borough select
    function handleBoroughSelect(e: Event){
        const target = e.target as HTMLSelectElement;
        selectedBorough = target.value;

        // set filtered list of coffee shops
        filteredCoffeeShopList = coffeeShopData.filter((shop) => shop.borough === selectedBorough);
        
        // calculate total revenue
        totalRevenue = filteredCoffeeShopList.reduce((acc, shop) => acc + shop.revenue, 0);
        
        // set hourly earnings
        hourlyEarnings = boroughData.find((borough) => borough.borough === selectedBorough)!.hourlyEarnings;
    }

    // fetch data when component mounts
    onMount(async () => {
        getCoffeeShopData();
        getBoroughData()
    });
</script>


<main class="flex flex-col items-center space-y-5">
    <div class="mx-auto">
        <img src="/ael-logo.png" alt="Amberside Energy Logo" class="mx-auto mt-4">
        <h1 class="text-3xl font-bold text-center my-4" id="main-heading" aria-label="Coffee Dashboard" >Coffee Dashboard</h1>

        <label class="text-xl font-bold" aria-label="Choose a borough" >Choose a borough: 
            <select bind:value={selectedBorough} on:change={handleBoroughSelect} class="focus:outline-none focus:shadow-outline text-right px-2">
                {#each boroughData as borough}
                    <option value={borough.borough}>{borough.borough}</option>
                {/each}
            </select>
        </label>
        {#if selectedBorough}
            <p>Borough: <span class="font-semibold pl-1">{selectedBorough}</span></p>
            <p>Total revenue: <span class="font-semibold pl-1">£{totalRevenue}</span></p>
            <p>Hourly Earnings: <span class="font-semibold pl-1">£{hourlyEarnings}</span></p>
        {/if}
    </div>

    <table class="border border-1 text-left rounded" aria-labelledby="main-heading">
        <thead class="bg-gray-50 [&>*]:px-2 border-b-2">
            <th class="w-[250px]" scope="col">Shop</th>
            <th class="w-[250px]" scope="col">Borough</th>
            <th class="w-[150px]" scope="col">Revenue (£)</th>
        </thead>
        <tbody>
            {#each filteredCoffeeShopList as shop}
                <tr class="[&>*]:px-2 border-b">
                    <td>{shop.shop}</td>
                    <td>{shop.borough}</td>
                    <td>{shop.revenue}</td>
                </tr>
            {/each}
        </tbody>
    </table>
</main>