<script>
  import Signup from "./lib/signup.svelte";
  import { Router, Route } from "svelte-routing";
  import Usernotfound from "./lib/usernotfound.svelte";
  import Home from "./lib/home.svelte";
  import Login from "./lib/login.svelte";
  import Dashboard from "./lib/dashboard.svelte";
  import DownloadCv from "./Routes/downloadCV.svelte";
  import DownloadLetter from "./Routes/downloadLetter.svelte";
  import AdminLogin from './lib/adminlogin.svelte';
  
  export let url = "";
</script>

<main>
  <Router {url}>
    <div>
      <Route path="/" component={Usernotfound} />
      <Route path="/signup" component={Signup} />
      <Route path="/:username" let:params
        ><Home username={params.username} /></Route
      >
      <Route path="/:username/manage" let:params
        ><Login username={params.username} /></Route
      >
      <Route path="/admin" let:params>
        <!-- <Dashboard username={params.username} operation="feedback" /> -->
        <AdminLogin username="admin"/>
      </Route>
        <Route path="/:username/manage/mycv" let:params>
          {#if localStorage.getItem(params.username)}
          <Dashboard username={params.username} operation="mycv" />
          {/if}
        </Route>
        <Route path="/:username/manage/mycover" let:params>
          {#if localStorage.getItem(params.username)}
          <Dashboard username={params.username} operation="mycover" />
          {/if}
        </Route>
        <Route path="/:username/manage/profile" let:params>
          {#if localStorage.getItem(params.username)}
          <Dashboard username={params.username} operation="profile" />
          {/if}
        </Route>
        <Route path="/:username/manage/password" let:params>
          {#if localStorage.getItem(params.username)}
          <Dashboard username={params.username} operation="password" />
          {/if}
        </Route>
        <Route path="/:username/manage/addcv" let:params>
          {#if localStorage.getItem(params.username)}
          <Dashboard username={params.username} operation="addcv" />
          {/if}
        </Route>
        <Route path="/:username/manage/addletter" let:params>
          {#if localStorage.getItem(params.username)}
          <Dashboard username={params.username} operation="addletter" />
          {/if}
        </Route>
        <Route path="/:username/manage/format" let:params>
          {#if localStorage.getItem(params.username)}
          <Dashboard username={params.username} operation="changeFormat" />
          {/if}
        </Route>
        <Route path="/:username/manage/feedback" let:params>
          {#if localStorage.getItem(params.username)}
          <Dashboard username={params.username} operation="feedback" />
          {/if}
        </Route>
        <Route path="/download/cv/:cvid/:format" let:params>
          <!-- {#if localStorage.getItem(params.username)} -->
          <!-- <Dashboard username={params.username} operation="addletter" /> -->
          <!-- {/if} -->
          <DownloadCv cvid={params.cvid} format={params.format}/>
        </Route>
        <Route path="/download/letter/:letterid/:format" let:params>
          <!-- {#if localStorage.getItem(params.username)} -->
          <!-- <Dashboard username={params.username} operation="addletter" /> -->
          <!-- {/if} -->
          <DownloadLetter letterid={params.letterid} format={params.format}/>
        </Route>
    </div>
  </Router>
</main>
