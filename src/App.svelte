<script>
  import Table from "./lib/Table.svelte";
  import OptionBar from "./lib/OptionBar.svelte";

  async function getPaintings() {
    let jsonFile = await fetch("./paintings.json");
    return await jsonFile.json();
  }

  let promise = getPaintings();

  let x = "";
  let y = "title";

  $: search_term = x.toLowerCase();
  $: field = y.toLowerCase();

</script>

<main class="w-full">
  {#await promise}
    <div class="grid content-center w-full h-screen justify-center">
      <h1 class="text-4xl">Load paintings...</h1>
    </div>
  {:then paintings}
    <OptionBar bind:title={x} bind:field={y} />
    <div class="max-w-full">
      <Table {paintings} filter={search_term} field={field}/>
    </div>
  {/await}
</main>
