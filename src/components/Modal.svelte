<script lang="ts">
  import { createEventDispatcher } from "svelte";

  type Button = {
    type?: "primary" | "secondary" | "link";
    text: string;
    disabled?: (() => boolean) | boolean;
  };

  export let size: number = 600;
  export let title: string = "Modal";
  export let text: string = "";
  export let primaryBtn: Button = {
    type: "primary",
    text: "Action",
    disabled: false,
  };
  export let secondaryBtn: Button = undefined;

  const dispatch = createEventDispatcher();
  let overlay: HTMLDivElement;

  const handleClickOutside = (evt: MouseEvent) => {
    if (evt.target === overlay) {
      closeModal();
    }
  };

  const closeModal = () => {
    dispatch("closeModal");
  };
</script>

<svelte:window on:click={handleClickOutside} />

<div id="overlay" bind:this={overlay}>
  <div id="content" style="width: {size}px !important">
    <h1>{title}</h1>
    <p>{text}</p>
    {#if secondaryBtn}
      <button class={secondaryBtn.type}>{secondaryBtn.text}</button>
    {/if}
    <button class={primaryBtn.type ?? "primary"}>{primaryBtn.text}</button>
  </div>
</div>

<style>
  #overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.6);
  }

  #content {
    background-color: #fefefe;
    margin: 15% auto;
    padding: 20px;
    border: 1px solid #888;
  }

  /* Extra small devices */
  @media (min-width: 320px) {
    /*  */
  }

  /* Small devices (landscape phones, 576px and up) */
  @media (min-width: 576px) {
    /*  */
  }

  /* Medium devices (tablets, 768px and up) */
  @media (min-width: 768px) {
    /*  */
  }

  /* Large devices (desktops, 992px and up) */
  @media (min-width: 992px) {
    /*  */
  }

  /* Extra large devices (large desktops, 1200px and up) */
  @media (min-width: 1200px) {
    /*  */
  }
</style>
