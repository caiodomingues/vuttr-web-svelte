<script lang="ts">
  import Tools from "./components/Tools.svelte";
  import Modal from "./components/Modal.svelte";

  let isChecked = false;
  let isTagOnly = false;
  let isModalActive = false;

  let icon = "plus";
  let title = "Add new tool";
  let primaryBtnText = "Add tool";

  let search = "";

  const handleSearch = (search: string) => {
    //
  };

  const handleAddTool = () => {
    console.log("submited");
  };
</script>

<main>
  <h1>VUTTR</h1>
  <h2>Very Useful Tools to Remember</h2>
  <nav>
    <div>
      <div id="search-container">
        <input
          type="text"
          name="Search"
          id="search"
          placeholder="Digite o que está procurando..."
          on:change={() => handleSearch(search)}
          bind:value={search}
        />
      </div>
      <div id="checkbox-container">
        <label for="tag-only">
          <input
            type="checkbox"
            name="Tag only"
            id="tag-only"
            bind:checked={isChecked}
            on:change={() => (isTagOnly = !isTagOnly)}
          />
          search in tags only
        </label>
      </div>
    </div>
    <button on:click={() => (isModalActive = true)}>+ Add</button>
  </nav>

  <Tools />

  {#if isModalActive}
    <Modal
      {icon}
      {primaryBtnText}
      {title}
      on:close={() => (isModalActive = false)}
      on:primaryBtn ={handleAddTool}
    >
      <h1>Hello, world!</h1>
    </Modal>
  {/if}
</main>

<style>
  main {
    padding: 100px 450px;
  }

  h1,
  h2 {
    color: #170c3a;
    font-style: normal;
    font-weight: 600;
    font-family: "Source Sans Pro", sans-serif;
  }

  h1 {
    font-size: 42px;
    line-height: 50px;
    letter-spacing: 0.84px;
  }

  h2 {
    font-size: 36px;
    line-height: 40px;
    letter-spacing: 0.72px;
  }

  nav {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }

  nav div {
    display: flex;
    /* flex: 1 1 600px; */
  }

  nav div #search-container {
    display: inline-flex;
    flex: 1 1 300px;
    position: relative;
    border: 1px solid #ebeaed;
    border-radius: 5px;
    overflow: hidden;
    background: #f5f4f6 0% 0% no-repeat padding-box;
    margin-right: 12px;
  }

  nav div #search-container input {
    border: 0;
    padding: 13px 0px 11px 13px;
    flex: 1;
    width: 100%;
    background: #f5f4f6 0% 0% no-repeat padding-box;

    text-align: left;
    font: normal normal normal 16px/20px "Source Sans Pro";
    letter-spacing: 0px;
    color: #170c3a;
  }

  nav div #checkbox-container {
    align-items: center;
  }

  nav div #checkbox-container label {
    text-align: left;
    font: normal normal normal 16px/20px "Source Sans Pro";
    letter-spacing: 0.4px;
    color: #170c3a;
  }

  nav div #tag-only {
    margin-right: 8px;
  }

  nav button {
    background: #365df0 0% 0% no-repeat padding-box;
    border-radius: 5px;
    border: none;

    text-align: center;
    font: normal normal 600 18px/24px Source Sans Pro;
    letter-spacing: 0.36px;
    color: #ffffff;
    padding: 14px 26px;

    cursor: pointer;
  }

  nav button:hover {
    background-color: #2f55cc;
  }

  nav button:active {
    background-color: #244aa8;
  }

  nav div input {
    text-align: left;
    background: #fff 0% 0% no-repeat padding-box;
    border: 1px solid #ebeaed;
    border-radius: 5px;
    padding: 13px 25px;
  }

  /* Extra small devices */
  @media (min-width: 320px) {
    main {
      padding: 100px 25px;
    }

    nav {
      flex-direction: column;
    }

    nav div {
      flex-direction: column;
    }

    nav div #checkbox-container {
      flex-direction: row;
      margin: 12px 0px;
    }

    nav div #search-container {
      display: flex;
      flex-direction: row;
      flex-basis: 50px;
    }
  }

  /* Small devices (landscape phones, 576px and up) */
  @media (min-width: 576px) {
    main {
      padding: 100px 50px;
    }

    nav div {
      flex: 1 1 10px;
    }

    nav div #tag-only {
      align-self: center;
    }

    nav div #search-container {
      flex: 1 1 50px;
    }
  }

  /* Medium devices (tablets, 768px and up) */
  @media (min-width: 768px) {
    main {
      padding: 100px 100px;
    }

    nav {
      flex-direction: row;
    }

    nav div {
      flex-direction: row;
      flex: 1 1 20px;
    }
  }

  /* Large devices (desktops, 992px and up) */
  @media (min-width: 992px) {
    main {
      padding: 100px 200px;
    }

    nav div {
      flex-basis: 300px;
    }
  }

  /* Extra large devices (large desktops, 1200px and up) */
  @media (min-width: 1200px) {
    main {
      padding: 100px 300px;
    }

    nav div {
      flex-basis: 400px;
    }
    nav div #search-container {
      flex: 1 1 400px;
    }
  }
</style>
