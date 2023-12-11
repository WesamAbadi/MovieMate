<script>
  import { Router, Route } from "svelte-routing";
  import About from "./about.svelte";
  import Page from "./+page.svelte";
  import Id from "./watch/[id].svelte";

  export let url = "";
</script>

<Router {url}>
  <div>
    <Route path="/" component={Page} />
    <Route path="/about" component={About} />
    <Route path="/watch/:id" component={Id} />
  </div>
</Router>
