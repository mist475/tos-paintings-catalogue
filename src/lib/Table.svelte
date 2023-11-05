<script>
  // @ts-nocheck

  export let paintings;
  export let filter;

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

    paintings = paintings.sort(compare_function);
  }

  //TODO: use the other text fields as well
  $: paintings = paintings_filter(paintings,filter)

  function paintings_filter(paintings, filter) {
    return paintings.filter((painting) => {
      console.log("runs")
      if (filter == "myName") {
        return true;
      }
      return painting.title.toLowerCase().includes(filter.toLowerCase());
    })
  }

  sort_function('title')
</script>
<p>Reactive value in the parent component: {filter}</p>
<table class="w-full p-6">
  <thead>
    <tr class="">
      <th class="">Painting</th>
      <th
        class=""
        on:click={() => {
          sort_function("title", true);
          sort_direction *= -1;
        }}>title</th
      >
      <th
        class=""
        on:click={() => {
          sort_function("width");
          sort_direction *= -1;
        }}>width</th
      >
      <th
        class=""
        on:click={() => {
          sort_function("height");
          sort_direction *= -1;
        }}>height</th
      >
      <th
        class=""
        on:click={() => {
          sort_function("lore_author", true);
          sort_direction *= -1;
        }}>lore author</th
      >
      <th
        class=""
        on:click={() => {
          sort_function("real_author", true);
          sort_direction *= -1;
        }}>real author</th
      >
    </tr>
  </thead>
  <tbody>
    {#each paintings as painting}
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
