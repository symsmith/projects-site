<script>
	import { Router } from "@sveltech/routify";
	import { routes } from "@sveltech/routify/tmp/routes";
</script>

<section class="section columns is-centered">
    <div class="container content column is-one-third">
      <Router {routes} />
    </div>
</section>