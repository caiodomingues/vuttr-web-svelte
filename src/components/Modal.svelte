<script lang="ts">
  import { createEventDispatcher } from "svelte";
  import IconClose from "../assets/img/icon-close.svelte";

  let props = {
    width: "15px",
    height: "15px",
    style: "align-self: center; margin-right: 6px;",
  };

  export let icon = "";
  export let title = "Modal";
  export let text = "";

  export let primaryBtnText = "Done";
  export let primaryBtnType = "primary";

  export let secondaryBtnText = undefined;
  export let secondaryBtnType = "secondary";

  const dispatch = createEventDispatcher();
  let overlay: HTMLDivElement;

  const handleClickOutside = (evt: MouseEvent) => {
    if (evt.target === overlay) {
      closeModal();
    }
  };

  const closeModal = () => {
    dispatch("close");
  };

  const primaryBtnAction = () => {
    dispatch("primaryBtn");
  };

  const secondaryBtnAction = () => {
    dispatch("secondaryBtn");
  };
</script>

<svelte:window on:click={handleClickOutside} />

<div id="overlay" bind:this={overlay}>
  <div id="content">
    <h1>
      {#if icon === "plus"}
        +
      {:else if icon === "clear"}
        <IconClose {props} />
      {/if}
      {title}
    </h1>
    <p>{text}</p>

    <slot />

    {#if primaryBtnText}
      <button on:click={primaryBtnAction} class={primaryBtnType}
        >{primaryBtnText}</button
      >
    {/if}

    {#if secondaryBtnType && secondaryBtnText}
      <button
        type="submit"
        on:click={secondaryBtnAction}
        class={secondaryBtnType}>{secondaryBtnText}</button
      >
    {/if}
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
    background: #ffffff 0% 0% no-repeat padding-box;
    padding: 16px;
    box-shadow: 0px 20px 25px #0000001a;
    border-radius: 5px;
    max-height: 100vh;
    overflow-y: auto;
    position: absolute;
    left: 50%;
    top: 50%;
    margin-top: -45vh;
  }

  #content h1 {
    color: #170c3a;
    font-style: normal;
    font-weight: 600;
    font-family: "Source Sans Pro", sans-serif;
    font-size: 26px;
    line-height: 32px;
    letter-spacing: 0.52px;
    margin: 0 12px 24px;
  }

  #content button {
    float: right;
  }
  #content button.danger {
    background: #f95e5a 0% 0% no-repeat padding-box;
    border-radius: 5px;
    text-align: center;
    font: normal normal 600 16px/20px "Source Sans Pro";
    letter-spacing: 0.36px;
    color: #ffffff;
    padding: 14px 18px;
    margin: 0;
  }

  #content button.danger:hover {
    background-color: #cc4c4c;
  }

  #content button.danger:active {
    background-color: #a53f3f;
  }

  #content button.secondary {
    background: #feefee 0% 0% no-repeat padding-box;
    border-radius: 5px;
    text-align: center;
    font: normal normal 600 16px/20px "Source Sans Pro";
    letter-spacing: 0.36px;
    color: #f95e5a;
    padding: 14px 18px;
    margin-right: 16px;
  }

  #content button.secondary:hover {
    background-color: #fcd7d6;
  }

  #content button.secondary:active {
    background-color: #fcc6c5;
  }

  /* Extra small devices */
  @media (min-width: 320px) {
    #content {
      width: 300px;
      margin-left: -150px;
    }
  }

  /* Small devices (landscape phones, 576px and up) */
  @media (min-width: 576px) {
    /*  */
  }

  /* Medium devices (tablets, 768px and up) */
  @media (min-width: 768px) {
    #content {
      width: 600px;
      margin-left: -300px;
    }
  }

  /* Large devices (desktops, 992px and up) */
  @media (min-width: 992px) {
  }

  /* Extra large devices (large desktops, 1200px and up) */
  @media (min-width: 1200px) {
    #content {
      width: 900px;
      margin-left: -450px;
    }
  }
</style>
