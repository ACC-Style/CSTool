<template>
	<div class="grid-x br-t-width_1 br_dashed br_primary padding-t_4">
		<div class="cell small-4">
			<a @click="$emit('email-postpend','@gmail.com')" class="button expanded secondaryalt m-b_0">
				<i class="fal fa-at"></i>gmail.com
			</a>
		</div>
		<div class="cell small-4">
			<a @click="$emit('email-postpend','@hotmail.com')" class="button expanded secondaryalt m-b_0">
				<i class="fal fa-at"></i>hotmail.com
			</a>
		</div>
		<div class="cell small-4">
			<a @click="$emit('email-postpend','@yahoo.com')" class="button expanded secondaryalt m-b_0">
				<i class="fal fa-at"></i>yahoo.com
			</a>
		</div>
		<div class="cell small-4">
			<a @click="$emit('email-postpend','@mail.com')" class="button expanded secondaryalt m-b_0">
				<i class="fal fa-at"></i>mail.com
			</a>
		</div>
		<div class="cell small-4">
			<a @click="$emit('email-postpend','@outlook.com')" class="button expanded secondaryalt m-b_0">
				<i class="fal fa-at"></i>outlook.com
			</a>
		</div>
		<div class="cell small-4">
			<a @click="$emit('email-postpend','@aol.com')" class="button expanded secondaryalt m-b_0">
				<i class="fal fa-at"></i>aol.com
			</a>
		</div>
	</div>
</template>

<script>
export default {};
</script>

<style scoped>
</style>