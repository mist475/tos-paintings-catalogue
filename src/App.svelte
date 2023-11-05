<script>
  import Table from "./lib/Table.svelte";
  import OptionBar from "./lib/OptionBar.svelte";

  async function getPaintings() {
    let jsonFile = await fetch("./paintings.json");
    return await jsonFile.json();
  }

  let promise = getPaintings();

  let p = 'MyName'
  $: data = p.toLowerCase()

  console.log(data)
</script>

<main class="w-full">
  {#await promise}
    <div class="grid content-center w-full h-screen justify-center">
      <h1 class="text-4xl">Load paintings...</h1>
    </div>
  {:then paintings}
    <div class="p-6">
      <OptionBar bind:title={p}/>
      <Table paintings={paintings} filter={data} />
    </div>
  {/await}
</main>
