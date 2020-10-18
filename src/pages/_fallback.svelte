<script>
    import { url, goto } from '@sveltech/routify'
    import {metatags} from '@sveltech/routify';
    metatags.title = "Projects - Page not found";

    $goto('/404');


</script>

<h1 class="title">404 - Page not found</h1>

<a href={$url('/')}>Go back</a>