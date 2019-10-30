<script>
  import { Link } from "svelte-routing";
</script>

<style>
  .brand-logo {
    font-size: 30px;
    font-weight: bold;
  }
</style>

<nav>
  <div class="nav-wrapper container">
    <Link href="#">
      <span class="brand-logo">Logo</span>
    </Link>
    <ul id="nav-mobile" class="right hide-on-med-and-down">
      <li>
        <Link to="/">Home</Link>
      </li>
      <li>
        <Link to="/about">About</Link>
      </li>
    </ul>
  </div>
</nav>
