<script>
  import { fade } from "svelte/transition";

  export let uri;
  export let colMap;
  export let filterFn = T;

  let spaceXResponse = [];
  const T = () => true;

  const p = fetch(uri)
    .then(res => res.json())
    .then(res => (spaceXResponse = res));

  $: launches = spaceXResponse.filter(filterFn);
</script>

<style type="text/scss">
  @import "./Table.scss";
</style>

<div class="table">
  {#await p}

    <p>Loading SpaceX launch data...</p>

  {:then value}

    <table>
      <thead>
        <tr>
          {#each colMap as prop}
            <th>{prop[1]}</th>
          {/each}
        </tr>
      </thead>
      <tbody>
        {#each launches as launch, index}
          <tr>
            {#each colMap as prop}
              <td>{launch[prop[0]] || ''}</td>
            {/each}
          </tr>
        {/each}
      </tbody>
    </table>

  {:catch error}

    <p>Ah damn... Something went wrong: {error.message}</p>

  {/await}

</div>
