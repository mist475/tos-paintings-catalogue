<script>
  //TODO rename
  export let title = "valueInChild";
  export let field = "title";
</script>

<div class="w-full bg-slate-900 p-3">
  <input
    type="text"
    id="myInput"
    class="rounded p-1 shadow-inner"
    placeholder="Search"
    bind:value={title}
  />
  <select class="p-1 shadow-inner rounded" bind:value={field}>
    <option value="title" > Title </option>
    <option value="width" > Width </option>
    <option value="height" > Height </option>
    <option value="lore_author" > Lore author </option>
    <option value="real_author" > Real author </option>

  </select>
</div>
