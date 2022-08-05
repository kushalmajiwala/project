<script>
  import { Router, Link, Route } from "svelte-routing";
  import About from './Routes/About.svelte';
  import AboutPost from './Routes/AboutPost.svelte';
  import Home from './Routes/Home.svelte';
  import {
    Button,
    Modal,
    ModalBody,
    ModalFooter,
    ModalHeader
  } from 'sveltestrap';
  let open = false;
  const toggle = () => (open = !open);
  export let url = "";
</script>
<main>
  <h1>This is our project...</h1>
  <h2>This is the Project Title</h2>
  <Router {url}>
    <nav>
      <Link to="/">Home</Link>
      <Link to="about">About</Link>
    </nav>
    <div>
      <Route path="about"><About /></Route> 
      <Route path="about/:id" let:params><AboutPost id={params.id} /></Route>
      <Route path="/"><Home /></Route>
    </div>
  </Router>
  <div>
    <Button color="danger" on:click={toggle}>Modal with no Backdrop</Button>
    <Modal isOpen={open} backdrop={false} {toggle}>
      <ModalHeader {toggle}>Modal title</ModalHeader>
      <ModalBody>
       Are you sure you want to delete this ?
      </ModalBody>
      <ModalFooter>
        <Button color="primary" on:click={toggle}>Cancel</Button>
        <Button color="secondary" on:click={toggle}>Delete</Button>
      </ModalFooter>
    </Modal>
  </div>
</main>