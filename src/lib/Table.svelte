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
    let normalizedFilter = normalize_string(filter);
    return paintings.filter((painting) => {
      //Show complete list if nothing entered
      if (normalizedFilter == undefined || normalizedFilter == '') {
        return true;
      }
      // Prevent type error
      if (field !== "width" && field !== "height") {
        return normalize_string(painting[field]).toLowerCase().includes(normalizedFilter.toLowerCase());
      } else {
        return painting[field] == normalizedFilter;
      }
    });
  }

  /**
   * Remove diacritics to make searching easier
   * Adapted from stackoverflow: https://stackoverflow.com/a/51874002
   */
  function normalize_string(input) {
    return input.normalize('NFD').replace(/\p{Diacritic}/gu, '');
  }

  //Sort list on title
  sort_function("title");
</script>

<div class="grid grid-cols-1">
  <div class="lg:grid lg:grid-cols-6  gap-3 p-2">
    <div class=" font-semibold invisible lg:visible lg:text-center">Painting</div>
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div
      class="font-semibold py-2 pl-2 lg:pl-0 lg:my-0 my-2  border-2 border-slate-700 rounded lg:text-center"
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
      class="font-semibold pl-2 lg:pl-0 lg:text-center lg:my-0 my-2 py-2  border-2 border-slate-700 rounded"
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
      class=" font-semibold pl-2 lg:pl-0 lg:text-center lg:my-0 my-2 py-2  border-2 border-slate-700 rounded"
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
      class="font-semibold pl-2 lg:pl-0 py-2 lg:text-center lg:my-0 my-2 border-2 border-slate-700 rounded"
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
      class="font-semibold pl-2 lg:pl-0 py-2 border-2 lg:text-center lg:my-0 my-2 border-slate-700 rounded "
      on:click={() => {
        sort_function("real_author", true);
        sort_direction *= -1;
      }}
    >
      Real author
    </div>
  </div>

  {#each displayed_paintings as painting}
    <div class="grid lg:grid-cols-6 gap-3 grid-cols-1 py-3 border-slate-900 border-y-2">
      <div><img src={painting.source} alt="" /></div>
      <div class="text-center">
        <div class="lg:hidden inline font-semibold ">Title:</div>
        {painting.title}
      </div>
      <div class="lg:text-center">
        <div class="lg:hidden inline font-semibold ">Width:</div>
        {painting.width}
      </div>
      <div class="lg:text-center">
        <div class="lg:hidden inline font-semibold ">Height:</div>
        {painting.height}
      </div>
      <div class="lg:text-center">
        <div class="lg:hidden inline font-semibold">Lore author:</div>
        {painting.lore_author}
      </div>
      <div class="lg:text-center">
        <div class="lg:hidden inline font-semibold ">Real author:</div>
        {painting.real_author}
      </div>
    </div>
  {/each}
</div>
