<script>
  import { onMount } from "svelte";
  import PostForm from "../components/PostForm.svelte";
  const start = 0,
    end = 10;
  let posts = [],
    editingPost = {
      title: "",
      body: "",
      userId: null,
      id: null
    };

  const apiBaseUrl = "https://jsonplaceholder.typicode.com/posts";

  onMount(async () => {
    const res = await fetch(`${apiBaseUrl}?_start=${start}&_limit=${end}`);
    posts = await res.json();
  });

  const addPost = event => {
    const { detail: postData } = event;
    if (posts.find(post => post.id === postData.id)) {
      const idx = posts.findIndex(p => p.id === postData.id);
      let postUpdated = posts;
      postUpdated.splice(idx, 1, postData);
      posts = postUpdated;
    } else posts = [postData[idx], ...posts];
  };

  const editPost = async post => {
    editingPost = post;
  };

  const deletePost = async id => {
    await fetch(`${apiBaseUrl}/${id}`, {
      method: "DELETE"
    });

    const newPost = posts.filter(post => post.id !== id);
    posts = newPost;
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
    <PostForm on:postCreated={addPost} {editingPost} />
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
            <a
              href="!#"
              class="btn"
              on:click|preventDefault={() => editPost(post)}>
              Edit
            </a>
            <a
              href="!#"
              class="btn delete-btn"
              on:click|preventDefault={() => deletePost(post.id)}>
              Delete
            </a>
          </div>
        </div>
      </div>
    {/each}
  {/if}
</div>
