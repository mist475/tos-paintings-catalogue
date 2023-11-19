<script>
  // @ts-nocheck

  export let paintings;
  export let filter;
  export let field;

  let displayed_paintings = paintings;
  $: displayed_paintings = paintings_filter(paintings, filter, field);

  let sort_direction = 1;

  function sort_function(column, is_text = false) {
    function compare_function(a, b) {
      if (is_text) {
        a = a[column].toLowerCase();
        b = b[column].toLowerCase();
      } else {
        a = a[column];
        b = b[column];
      }

      if (a < b) {
        return sort_direction;
      } else if (a > b) {
        return -1 * sort_direction;
      }

      return 0;
    }

    displayed_paintings = displayed_paintings.sort(compare_function);
  }

  function paintings_filter(paintings, filter, field) {
    console.log([filter, field]);

    return paintings.filter((painting) => {
      //Show complete list if nothing entered
      if ((filter == undefined) | (filter == "")) {
        return true;
      }

      // Prevent type error
      if (field !== "width" && field !== "height") {
        return painting[field].toLowerCase().includes(filter.toLowerCase());
      } else {
        return painting[field] == filter;
      }
    });
  }

  //Sort list on title
  sort_function("title");
</script>

<div class="grid grid-cols-1">
  <div class="lg:grid grid-cols-6 hidden gap-3 p-2">
    <div class=" font-semibold">Painting</div>
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div
      class="font-semibold"
      on:click={() => {
        sort_function("title", true);
        sort_direction *= -1;
      }}
    > 
      Title
    </div>
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div
      class="text-center font-semibold"
      on:click={() => {
        sort_function("width");
        sort_direction *= -1;
      }}
    >
      Width
    </div>
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div
      class=" font-semibold text-center"
      on:click={() => {
        sort_function("height");
        sort_direction *= -1;
      }}
    >
      Height
    </div>
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div
      class="font-semibold"
      on:click={() => {
        sort_function("lore_author", true);
        sort_direction *= -1;
      }}
    >
      Lore author
    </div>
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div
      class="font-semibold"
      on:click={() => {
        sort_function("real_author", true);
        sort_direction *= -1;
      }}
    >
      Real author
    </div>
  </div>

  {#each displayed_paintings as painting}
    <div class="grid lg:grid-cols-6 gap-3 grid-cols-1">
      <div><img src={painting.source} alt="" /></div>
      <div>
        <div class="lg:hidden inline font-semibold">title:</div>
        {painting.title}
      </div>
      <div class="lg:text-center">
        <div class="lg:hidden inline font-semibold">width:</div>
        {painting.width}
      </div>
      <div class="lg:text-center">
        <div class="lg:hidden inline font-semibold">height:</div>
        {painting.height}
      </div>
      <div>
        <div class="lg:hidden inline font-semibold">lore author:</div>
        {painting.lore_author}
      </div>
      <div>
        <div class="lg:hidden inline font-semibold">real author:</div>
        {painting.real_author}
      </div>
    </div>
  {/each}
</div>
