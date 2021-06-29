<script lang="ts">
  import Card from "./Card.svelte";

  type Tool = {
    id: number;
    title: string;
    link: string;
    description: string;
    tags: Array<string>;
  };

  const getTools = async (): Promise<Tool[]> => {
    let data = await fetch("http://localhost:8000/api/tools");
    let res = await data.json();

    if (data.ok) {
      return res.data;
    } else {
      throw new Error(res.message_raw);
    }
  };

  const handleToolDelete = async ({ id }: Tool): Promise<any> => {
    let data = await fetch(`http://localhost:8000/api/tools/${id}`, {
      method: "DELETE",
    });
    let res = await data.json();

    if (data.ok) {
      return allTools;
    } else {
      throw new Error(res.message_raw);
    }
  };

  $: allTools = getTools();
</script>

<section>
  {#await allTools}
    <p>Loading...</p>
  {:then tools}
    {#each tools as tool}
      <Card {tool} on:click={() => handleToolDelete(tool)} />
    {/each}
  {:catch error}
    <p style="color: red">{error.message}</p>
  {/await}
</section>
