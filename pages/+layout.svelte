<script>
	import '@evidence-dev/tailwind/fonts.css';
	import '../app.css';
	import { base } from '$app/paths';
	import { EvidenceDefaultLayout } from '@evidence-dev/core-components';
	export let data;
</script>

<EvidenceDefaultLayout {data} logo={`${base}/logo.png`}>
	<slot slot="content" />
</EvidenceDefaultLayout>
