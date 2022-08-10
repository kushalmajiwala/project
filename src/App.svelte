<script>
  import Signup from './lib/signup.svelte';
  import { Router, Route } from "svelte-routing";
  import Usernotfound from './lib/usernotfound.svelte';
  import Home from './lib/home.svelte';
  import Login from './lib/login.svelte';
  export let url = "";
</script>
<main>
  <Router url="{url}">
    <div>
      <Route path="/" component="{Usernotfound}" />
      <Route path="/signup" component="{Signup}" />
      <Route path="/:username" component="{Home}" />
      <Route path="/:username/manage" component="{Login}"/>
    </div>
  </Router>
</main>