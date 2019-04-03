<template>
	<div id="reveal-dialog" class="reveal large" data-reveal data-animate-out="slide-out-down">
		<button class="close-button" data-close aria-label="Close modal" type="button">
			<span aria-hidden="true">
				<i class="fas fa-times"></i>
			</span>
		</button>
		<slot name="header">default header</slot>
		<slot name="content">
			<h1>Awesome. I Have It.</h1>
			<p class="lead">Your couch. It is mine.</p>
			<p>I'm a cool paragraph that lives inside of an even cooler modal. Wins!</p>
		</slot>
	</div>
</template>

<script>
export default {
	name: "reveal",
	mounted() {
		this.reveal = new Foundation.Reveal($("#reveal-dialog"), {
			// These options can be declarative using the data attributes
			animationIn: "slide-in-up",
			animationOut: "slide-out-down"
		});
	},
	data() {
		return {
			msg: "Reveal"
		};
	},
	methods: {
		// Added the below openReveal method for two reasons:
		// 1) There was a bug preventing the reveal from working once
		// you navigated away and back to the reveal component.
		// 2) Most dialogs will need to be opened using code.
		openReveal: function() {
			this.reveal.open();
		},
		closeReveal: function() {
			this.reveal.close();
		}
	},
	destroyed() {
		this.reveal.destroy();
	}
};
</script>

<style lang="scss" scoped>
.reveal {
	display: block;
	height: 50vh;
	min-height: 50vh;
	top: unset !important;
	bottom: 0 !important;
	position: fixed;
	width: 100%;
	max-width: none;
}
</style>
