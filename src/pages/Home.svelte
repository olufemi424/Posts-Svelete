<script>
  import { onMount } from "svelte";
  import PostForm from "../components/PostForm.svelte";
  let posts = [];

  const apiBaseUrl = "https://jsonplaceholder.typicode.com/posts";
  onMount(async () => {
    const res = await fetch(apiBaseUrl);
    posts = await res.json();
  });

  const editPost = post => {
    console.log(post);
  };

  const addPost = event => {
    posts = [event.detail, ...posts];
  };

  const deltePost = id => {
    console.log(id);
  };

  const reduce = (body, start = 0, end) => {
    if (body) return body.slice(start, end);
  };
</script>

<style>
  .delete-btn {
    background: rgb(188, 29, 29);
  }
  .card .card-content .card-title {
    margin-bottom: 0;
  }
</style>

<div class="row">
  <div class="col s6">
    <PostForm on:postCreated={addPost} />
  </div>
</div>
<div class="row">
  {#if posts.length === 0}
    <h3>Loading...</h3>
  {:else}
    {#each posts as post}
      <div class="col s6">
        <div class="card">
          <div class="card-content">
            <p class="card-title">{reduce(post.title, 0, 20)}</p>
            <p>{reduce(post.body, 0, 200)}</p>
          </div>
          <div class="card-action">
            <a href="!#" class="btn" on:click={() => editPost(post)}>Edit</a>
            <a
              href="!#"
              class="btn delete-btn"
              on:click={() => deltePost(post.id)}>
              Delete
            </a>
          </div>
        </div>
      </div>
    {/each}
  {/if}
</div>
