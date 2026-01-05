<script setup lang="ts">
import { TresCanvas } from '@tresjs/core'
import { BasicShadowMap, NoToneMapping, SRGBColorSpace } from 'three'
import OrbbScene from './OrbScene.vue'

export type AgentState = null | 'thinking' | 'listening' | 'talking'

interface OrbProps {
	colors?: [string, string]
	colorsRef?: { value: [string, string] }
	resizeDebounce?: number
	seed?: number
	agentState?: AgentState
	volumeMode?: 'auto' | 'manual'
	manualInput?: number
	manualOutput?: number
	inputVolumeRef?: { value: number }
	outputVolumeRef?: { value: number }
	getInputVolume?: () => number
	getOutputVolume?: () => number
	className?: string
}

const props = withDefaults(defineProps<OrbProps>(), {
	colors: () => ['#CADCFC', '#A0B9D1'],
	resizeDebounce: 100,
	volumeMode: 'auto',
	agentState: null,
})
</script>

<template>
	<div :class="className ?? 'relative h-full w-full'">
		<TresCanvas
			:resize="{ debounce: resizeDebounce }"
			:clear-color="'#ffffff'"
			:gl="{
				alpha: true,
				antialias: true,
				premultipliedAlpha: true,
				// toneMapping: NoToneMapping,
				// outputColorSpace: SRGBColorSpace,
			}"
		>
			<TresPerspectiveCamera :position="[0, 0, 5]" :fov="75" />
			<Suspense>
				<OrbbScene
					:colors="colors"
					:colors-ref="colorsRef"
					:seed="seed"
					:agent-state="agentState"
					:volume-mode="volumeMode"
					:manual-input="manualInput"
					:manual-output="manualOutput"
					:input-volume-ref="inputVolumeRef"
					:output-volume-ref="outputVolumeRef"
					:get-input-volume="getInputVolume"
					:get-output-volume="getOutputVolume"
				/>
			</Suspense>
		</TresCanvas>
	</div>
</template>
