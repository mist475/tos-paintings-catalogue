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

<table class="w-full table-auto">
  <thead>
    <tr>
      <th class="">Painting</th>
      <th
        class=""
        on:click={() => {
          sort_function("title", true);
          sort_direction *= -1;
        }}>Title</th
      >
      <th
        class="text-center"
        on:click={() => {
          sort_function("width");
          sort_direction *= -1;
        }}>Width</th
      >
      <th
        class=""
        on:click={() => {
          sort_function("height");
          sort_direction *= -1;
        }}>Height</th
      >
      <th
        class=""
        on:click={() => {
          sort_function("lore_author", true);
          sort_direction *= -1;
        }}>Lore author</th
      >
      <th
        class=""
        on:click={() => {
          sort_function("real_author", true);
          sort_direction *= -1;
        }}>Real author</th
      >
    </tr>
  </thead>
  <tbody>
    {#each displayed_paintings as painting}
      <tr>
        <td><img src={painting.source} alt="" /></td>
        <td>{painting.title}</td>
        <td>{painting.width}</td>
        <td>{painting.height}</td>
        <td>{painting.lore_author}</td>
        <td>{painting.real_author}</td>
      </tr>
    {/each}
  </tbody>
</table>
