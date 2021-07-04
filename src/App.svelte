<script lang="ts">
  import Tools from "./components/Tools.svelte";
  import Modal from "./components/Modal.svelte";

  type Tool = {
    id: number;
    title: string;
    link: string;
    description: string;
    tags: Array<string>;
  };

  let isTagOnly = false;
  let isModalActive = false;

  let icon = "plus";
  let title = "Add new tool";
  let primaryBtnText = "Add tool";

  let search = "";

  let name = "";
  let link = "";
  let description = "";
  let tags = "";
  let errorTag = "";

  let tools: { getTools: () => Promise<Tool[]> };
  let allTools: Promise<Tool[]>;

  const handleSearch = async () => {
    // Haven't done yet, because the way I treated componentization and reactivity at this app doesn't help me,
    // it would be better if I passed Tools data through the child, like this:
    // <Tools {tools} />
  };

  const handleAddTool = async (evt: CustomEvent) => {
    evt.preventDefault();

    let data = await fetch(`http://localhost:8000/api/tools`, {
      method: "POST",
      headers: {
        Accept: "application/json",
        "Content-type": "application/json",
      },
      body: JSON.stringify({
        title: name,
        link,
        description,
        tags: tags.split(" "),
      }),
    });
    let res = await data.json();

    if (data.ok) {
      allTools = tools.getTools();
      isModalActive = false;
      name = "";
      link = "";
      description = "";
      tags = "";
      errorTag = "";
    } else {
      if (res.message_raw.slice(0, 13) === "Tag not found") {
        errorTag = res.message_raw;
      }

      throw new Error(res.message);
    }
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
          on:input={handleSearch}
          bind:value={search}
        />
      </div>
      <div id="checkbox-container">
        <label for="tag-only">
          <input
            type="checkbox"
            name="Tag only"
            id="tag-only"
            bind:checked={isTagOnly}
            on:input={handleSearch}
          />
          search in tags only
        </label>
      </div>
    </div>
    <button on:click={() => (isModalActive = true)}>+ Add</button>
  </nav>

  <Tools {allTools} bind:this={tools} />

  {#if isModalActive}
    <Modal
      {icon}
      {title}
      {primaryBtnText}
      on:close={() => (isModalActive = false)}
      on:primaryBtn={(evt) => handleAddTool(evt)}
    >
      <form>
        <div class="input-group">
          <label for="toolName">
            Tool name <span class="required">*</span>
          </label>
          <input
            bind:value={name}
            type="text"
            name="tool name"
            id="toolName"
            required
            autoComplete="off"
            minLength={1}
            maxLength={25}
          />
        </div>
        <div class="input-group">
          <label for="toolLink">
            Tool link <span class="required">*</span>
          </label>
          <input
            bind:value={link}
            type="text"
            name="tool link"
            id="toolLink"
            autoComplete="off"
            required
            minLength={1}
            maxLength={200}
          />
        </div>
        <div class="input-group">
          <label for="toolName">
            Tool description <span class="required">*</span>
          </label>
          <textarea
            bind:value={description}
            rows={4}
            name="tool description"
            id="toolDescription"
            required
            maxLength={255}
          />
        </div>
        <div class="input-group">
          <label for="tags">
            Tags <span class="required">*</span>
          </label>
          <input
            bind:value={tags}
            type="text"
            name="tags"
            id="tags"
            class:inputError={errorTag}
            autoComplete="off"
            required
            minLength={1}
            maxLength={255}
          />
          {#if errorTag}
            <p class="error">{errorTag}</p>
          {/if}
        </div>
      </form>
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

  form label {
    color: #170c3a;
    font-style: normal;
    font-weight: 600;
    font-family: "Source Sans Pro", sans-serif;
  }

  form input {
    background: #f5f4f6 0% 0% no-repeat padding-box;
    border: 1px solid #ebeaed;
    border-radius: 5px;
    padding: 13px 22px;

    font: normal normal normal 16px/20px "Source Sans Pro";
    letter-spacing: 0px;
    color: #170c3a;
  }

  form textarea {
    background: #f5f4f6 0% 0% no-repeat padding-box;
    border: 1px solid #ebeaed;
    border-radius: 5px;
    resize: none;
    padding: 11px 21px;

    font: normal normal normal 16px/20px "Source Sans Pro";
    letter-spacing: 0px;
    color: #170c3a;
  }

  form .input-group {
    display: flex;
    flex-direction: column;
    padding: 0px 30px;
    margin-bottom: 24px;
  }

  form .error {
    text-align: right;
    font: normal normal normal 16px/20px "Source Sans Pro";
    letter-spacing: 0.36px;
    color: #f95e5a;
  }

  form .inputError {
    background: #feefee 0% 0% no-repeat padding-box;
    border: 1px solid #f95e5a;
    color: #f95e5a;
    border-radius: 5px;
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
