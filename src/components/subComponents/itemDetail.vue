<template>
	<div class="grid-x grid-frame">
		<div class="cell shrink p_1 p-t_2">
			<i class="m-l_3 m-r_3 c_secondary-2 fal font_4:md font_2" v-bind:class="icon"></i>
		</div>
		<div class="cell auto lh_1">
			<div class="c_secondary-3 font_n2 d_none d_inline-block:md">{{label}}</div>
			<div class="c_secondary-n4 font_1 font_2:md m-t_3 m-t_0:md overflow-ellipsis p-b_3">
				<span v-if="!mask">{{detailData}}</span>
				<span v-if="mask">******</span>
			</div>
		</div>
		<div class="cell shrink" v-if="mask">
			<a href="#" class="link p-t_2 p-r_3 p_1 font_1 font_3:md">
				<i class="fal fa-eye"></i>
			</a>
		</div>
		<div class="cell shrink" v-if="editable">
			<a href="#" class="link p-t_2 p-r_3 p_1 font_1 font_3:md" @click="$emit('emitBasic')">
				<i class="fal fa-pencil"></i>
			</a>
		</div>
	</div>
</template>

<script>
export default {
	props: {
		label: { type: String, default: "label" },
		emitString: { type: String, default: "open-username-reveal" },
		detailData: { type: String, default: "data" },
		icon: { type: String, default: "fa-gear" },
		editable: { type: Boolean, default: false },
		mask: { type: Boolean, default: false }
	}
};
</script>

<style scoped>
</style>