<script lang="ts">
    import type { PageData } from './$types';

	export let data: PageData;

    let progress = 1;
    let percentage = 0;

    data.instances.forEach(p => {
        p.then(() => {
            percentage = progress++ / data.total * 100;
        })

        p.catch(() => {
            percentage = progress++ / data.total * 100;
        })
    });
</script>

<svelte:head>
	<title>Info on {data.name}</title>
	<meta name="description" content="">
</svelte:head>

Scanned: {percentage.toFixed(2) + "%"} {progress - 1} / {data.total}

<br>


<div style="margin-top: 4rem;">
    The following instances have blocked <span>{data.name}</span>
    <ul>
        {#each data.instances as instance}
            {#await instance then inst}
                {#if inst.blocked}
                    <li>
                        {inst.name}
                    </li>
                {/if}
            {/await}
        {/each}
    </ul>
</div>