<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  //import variables
  export let editingPost;

  const apiBaseUrl = "https://jsonplaceholder.typicode.com/posts";
  $: title = editingPost.title;
  $: body = editingPost.body;
  let loading = false;

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

    let method, url;

    if (editingPost.id) {
      url = `${apiBaseUrl}/${editingPost.id}`;
      method = "PUT";
    } else {
      url = `${apiBaseUrl}`;
      method = "POST";
    }

    const res = await fetch(url, {
      method: method,
      body: JSON.stringify(newPost),
      headers: {
        "Content-type": "application/json; charset=UTF-8"
      }
    });

    const post = await res.json(); // something wrong with api

    dispatch("postCreated", post); // dispatch to parent component

    if (res.ok) {
      console.log("Post Created 👍🏿");
      editingPost.title = "";
      editingPost.body = "";
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
      <input type="text" bind:value={editingPost.title} />
    </div>
    <div class="input-field">
      <label for="body">Body</label>
      <textarea bind:value={editingPost.body} class="materialize-textarea" />
    </div>
    <button type="submit" class="waves-effect waves-light btn">
      {editingPost.id ? 'Update' : 'Add'}
    </button>
  </form>
{:else}
  <div class="progress">
    <div class="indeterminate" />
  </div>
{/if}
