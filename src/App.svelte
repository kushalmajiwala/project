<script>
  import Signup from './lib/signup.svelte';
  import { Router, Link, Route } from "svelte-routing";
  export let url = "";
</script>
<main>
  <Router url="{url}">
    <div>
      <Route path="signup" component="{Signup}" />
    </div>
  </Router>
</main>