<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  const apiBaseUrl = "https://jsonplaceholder.typicode.com/posts";
  $: title = "";
  $: body = "";
  $: loading = false;

  const onSubmit = async event => {
    event.preventDefault();
    if (title.trim() === "" || body.trim() === "") {
      return;
    }

    loading = true;

    const newPost = {
      title,
      body
    };

    const res = await fetch(`${apiBaseUrl}`, {
      method: "POST",
      body: JSON.stringify(newPost),
      headers: {
        "Content-type": "application/json; charset=UTF-8"
      }
    });

    const post = await res.json(); // something wrong with api

    dispatch("postCreated", post); // dispatch to parent component

    if (res.ok) {
      console.log("Post Created 👍🏿");
      title = "";
      body = "";
    }
    loading = false;
  };
</script>

<style>
  form {
    margin: 50px;
  }

  .progress {
    margin: 50px auto;
  }
</style>

{#if !loading}
  <form on:submit={onSubmit}>
    <div class="input-field">
      <label for="title">Title</label>
      <input type="text" bind:value={title} />
    </div>
    <div class="input-field">
      <label for="body">Body</label>
      <textarea bind:value={body} class="materialize-textarea" />
    </div>
    <button type="submit" class="waves-effect waves-light btn">Add</button>
  </form>
{:else}
  <div class="progress">
    <div class="indeterminate" />
  </div>
{/if}
