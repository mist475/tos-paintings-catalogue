<script>
  import Table from "./lib/Table.svelte";
  import Sortbar from "./lib/sortbar.svelte";

  async function getPaintings() {
    let jsonFile = await fetch("./paintings.json");
    let json = await jsonFile.json();



    return json;
  }

  let promise = getPaintings();
</script>

<main class="w-full">
  {#await promise}
    <div class="grid content-center w-full h-screen justify-center">
      <h1 class="text-4xl">Load paintings...</h1>
    </div>
  {:then paintings}
    <div class="p-6">
      <Sortbar/>
      <Table {paintings} />
    </div>
  {/await}
</main>
