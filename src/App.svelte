<script>
  import Signup from './lib/signup.svelte';
  import { Router, Route } from "svelte-routing";
  import Usernotfound from './lib/usernotfound.svelte';
  import Home from './lib/home.svelte';
  import Login from './lib/login.svelte';
  import Dashboard from './lib/dashboard.svelte';
  export let url = "";
</script>
<main>
  <Router url="{url}">
    <div>
      <Route path="/" component="{Usernotfound}" />
      <Route path="/signup" component="{Signup}" />
      <Route path="/:username" let:params><Home username={params.username} /></Route>
      <Route path="/:username/manage" let:params><Login username={params.username} /></Route>
      <Route path="/:username/manage/mycv" let:params><Dashboard username={params.username} operation="mycv" /></Route>
      <Route path="/:username/manage/mycover" let:params><Dashboard username={params.username} operation="mycover" /></Route>
      <Route path="/:username/manage/profile" let:params><Dashboard username={params.username} operation="profile" /></Route>
      <Route path="/:username/manage/password" let:params><Dashboard username={params.username} operation="password" /></Route>
    </div>
  </Router>
</main>