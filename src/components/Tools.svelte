<script lang="ts">
  import Card from "./Card.svelte";
  import Modal from "./Modal.svelte";

  type Tool = {
    id: number;
    title: string;
    link: string;
    description: string;
    tags: Array<string>;
  };

  let selectedTool: Tool;
  let isModalActive = false;

  let icon = "clear";
  let title = "Remove tool";
  let text = "Are you sure you want to remove";
  let primaryBtnText = "Remove";
  let primaryBtnType = "danger";
  let secondaryBtnText = "Cancel";

  const getTools = async (): Promise<Tool[]> => {
    let data = await fetch("http://localhost:8000/api/tools");
    let res = await data.json();

    if (data.ok) {
      return res.data;
    } else {
      throw new Error(res.message_raw);
    }
  };

  const handleOpenModal = (tool: Tool) => {
    isModalActive = true;
    selectedTool = tool;
  };

  const handleRemoveTool = async () => {
    let data = await fetch(
      `http://localhost:8000/api/tools/${selectedTool.id}`,
      {
        method: "DELETE",
      }
    );

    isModalActive = false;
    if (data.ok) {
      return allTools;
    } else {
      throw new Error("Error");
    }
  };

  $: allTools = getTools();
</script>

<section>
  {#await allTools}
    <p>Loading...</p>
  {:then tools}
    {#each tools as tool}
      <Card {tool} on:click={() => handleOpenModal(tool)} />
    {/each}
  {:catch error}
    <p style="color: red">{error.message}</p>
  {/await}
</section>

{#if isModalActive}
  <Modal
    {secondaryBtnText}
    {primaryBtnText}
    {primaryBtnType}
    {title}
    {icon}
    text={`${text} ${selectedTool.title}?`}
    on:primaryBtn={handleRemoveTool}
    on:secondaryBtn={() => (isModalActive = false)}
    on:close={() => (isModalActive = false)}
  />
{/if}
