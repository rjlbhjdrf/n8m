<template>
	<div>
		<slot v-if="canAccess" />
		<slot v-else name="fallback" />
	</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import type { EnterpriseEditionFeature } from '@/constants';
import { mapStores } from 'pinia';
import { useSettingsStore } from '@/stores/settings.store';

export default defineComponent({
	name: 'EnterpriseEdition',
	props: {
		features: {
			type: Array,
			default: () => [] as EnterpriseEditionFeature[],
		},
	},
	computed: {
		...mapStores(useSettingsStore),
		canAccess(): boolean {
			return this.features.reduce((acc: boolean, feature) => {
				return (
					acc &&
					!!this.settingsStore.isEnterpriseFeatureEnabled(feature as EnterpriseEditionFeature)
				);
			}, true);
		},
	},
});
</script>
