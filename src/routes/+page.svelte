<script>
	import Monitor from "$lib/components/monitor.svelte";
	import * as Card from "$lib/components/ui/card";
	import { Button, buttonVariants } from "$lib/components/ui/button";
	import Incident from "$lib/components/incident.svelte";
	import { Badge } from "$lib/components/ui/badge";
	import { l } from "$lib/i18n/client";
	import { base } from "$app/paths";

	export let data;
	let hasActiveIncidents = data.openIncidents.length > 0;
</script>

<div class="mt-32"></div>
{#if data.site.hero}
	<section class="mx-auto mb-8 flex w-full max-w-4xl flex-1 flex-col items-start justify-center">
		<div class="mx-auto max-w-screen-xl px-4 lg:flex lg:items-center">
			<div class="blurry-bg mx-auto max-w-3xl text-center">
				{#if data.site.hero.image}
					<img src={data.site.hero.image} class="m-auto h-16 w-16" alt="" srcset="" />
				{/if}
				{#if data.site.hero.title}
					<h1
						class="bg-gradient-to-r from-white bg-clip-text text-5xl"
					>
						{data.site.hero.title}
					</h1>
				{/if}
				{#if data.site.hero.subtitle}
					<p class="mx-auto mt-4 max-w-xl sm:text-xl">{data.site.hero.subtitle}</p>
				{/if}
			</div>
		</div>
	</section>
{/if}
{#if hasActiveIncidents}
	<section
		class="mx-auto mb-4 flex w-full max-w-[890px] flex-1 flex-col items-start justify-center bg-transparent"
		id=""
	>
		<div class="grid w-full grid-cols-2 gap-4">
			<div class="col-span-2 text-center md:col-span-1 md:text-left">
				<Badge variant="outline">
					{l(data.lang, "root.ongoing_incidents")}
				</Badge>
			</div>
		</div>
	</section>
	<section
		class="mx-auto mb-8 flex w-full max-w-[890px] flex-1 flex-col items-start justify-center backdrop-blur-[2px]"
		id=""
	>
		{#each data.openIncidents as incident, i}
			<Incident
				{incident}
				state="close"
				variant="title+body+comments+monitor"
				monitor={incident.monitor}
				lang={data.lang}
			/>
		{/each}
	</section>
{/if}
{#if data.monitors.length > 0}
	<section
		class="mx-auto mb-4 flex w-full max-w-[890px] flex-1 flex-col items-start justify-center bg-transparent"
		id=""
	>
		<div class="grid w-full grid-cols-2 gap-4">
			<div class="col-span-2 text-center md:col-span-1 md:text-left">
				<Badge class="" variant="outline">
					{l(data.lang, "root.availability_per_component")}
				</Badge>
			</div>
			<div class="col-span-2 text-center md:col-span-1 md:text-right">
				<Badge variant="outline">
					<span class="bg-api-up mr-1 inline-flex h-[8px] w-[8px] rounded-full opacity-75"
					></span>
					<span class="mr-3">
						{l(data.lang, "statuses.UP")}
					</span>

					<span
						class="bg-api-degraded mr-1 inline-flex h-[8px] w-[8px] rounded-full opacity-75"
					></span>
					<span class="mr-3">
						{l(data.lang, "statuses.DEGRADED")}
					</span>

					<span
						class="bg-api-down mr-1 inline-flex h-[8px] w-[8px] rounded-full opacity-75"
					></span>
					<span class="mr-3">
						{l(data.lang, "statuses.DOWN")}
					</span>
				</Badge>
			</div>
		</div>
	</section>
	<section
		class="mx-auto mb-8 flex w-full max-w-[890px] flex-1 flex-col items-start justify-center backdrop-blur-[2px]"
	>
		<Card.Root>
			<Card.Content class="monitors-card p-0">
				{#each data.monitors as monitor}
					<Monitor {monitor} localTz={data.localTz} lang={data.lang} />
				{/each}
			</Card.Content>
		</Card.Root>
	</section>
{/if}
{#if data.site.categories}
	<section
		class="mx-auto mb-8 w-full max-w-[890px] flex-1 flex-col items-start backdrop-blur-[2px]"
	>
		<h2 class="mb-2 mt-2 px-2 text-xl font-semibold">
			{l(data.lang, "root.other_monitors")}
		</h2>
		{#each data.site.categories as category}
			<Card.Root class="mb-2 w-full">
				<Card.Header>
					<Card.Title>{category.name}</Card.Title>
					<Card.Description class="relative pr-[100px]">
						{#if category.description}
							{category.description}
						{/if}
						<a
							href="{base}/category-{category.name}"
							class="{buttonVariants({
								variant: 'secondary'
							})} absolute -top-4 right-2"
						>
							View
						</a>
					</Card.Description>
				</Card.Header>
			</Card.Root>
		{/each}
	</section>
{/if}
