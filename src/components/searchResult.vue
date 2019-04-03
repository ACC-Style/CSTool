<template>
	<div class="grid-y" style="z-index:-1">
		<div
			class="searchResults result card shadow_1 br_radius m_3 br_secondary-4"
			v-bind:class="{ 'selectable bg_warning-4 br_warning br_solid br_2': selectable }"
		>
			<div class="select-container bg_warning-n3" @click="select_toggle">
				<div class="p_3 relative">
					<i class="far fa-circle c_white fa-crown"></i>
					<div v-if="selectable" class="c_white nowrap relative t_4 r_0 font_bold">
						<span class="turn90 absolute" style="bottom: -4.5rem; left: -3.5rem;">Primary Account</span>
					</div>
				</div>
			</div>
			<div class="detail-container grid-frame">
				<div class="grid-x">
					<div class="cell auto">
						<div
							class="member-name font_1 font_2:md font_3:lg font_slab line-height_2 member-name p_3 p-l_4 p-r_3"
						>
							{{fullName}}
							<a
								@click="$emit('share-record')"
								class="bg_secondary-4 float-right font_0 hover:bg_secondary p-l_3 p-r_3 p_2"
							>
								<i class="fas fa-share-alt-square"></i>
							</a>
						</div>
						<div
							class="d_block:md d_none font_n1 c_primary-n2 font_bold p-l_2 m-t_n4 grid-condtainer br-b_1 br_secondary-3 br_solid"
						>
							<div class="p_2 grid-x">
								<div class="cell shrink" v-if="hasMembership">
									<i class="m-l_3 m-r_3 c_primary-1 fas fa-user-md"></i>
									{{memberType}}
								</div>
								<div class="cell shrink" v-if="!hasMembership">
									<i class="m-l_3 m-r_3 c_primary-1 fas fa-user-slash"></i> NPI
								</div>
								<div class="cell auto p-l_2 br-l_2 br_secondary-5 br_solid m-l_3">
									<i class="m-l_3 m-r_3 c_primary-1 fas fa-hashtag"></i>
									{{personifyNumber}}
								</div>
							</div>
						</div>
						<div class>
							<ul class="details no-bullet p-l_3 p-r_3 m-b_0 font_0 grid-x m-t_3:lg">
								<li
									class="br-b_1 br_0 br_secondary-5 br_solid p_2 p-l_0 p-b_0 p-b_2:md cell p-r_5:lg small-12 large-6 d_block:sm d_none:md"
								>
									<div class="grid-x grid-frame">
										<div class="cell shrink">
											<itemDetail
												:label="'member'"
												:detailData="memberTypeComputed"
												:icon="memberIconComputed"
												:editable="false"
											></itemDetail>
										</div>
										<div class="cell shrink p-r_3">
											<itemDetail
												:label="'personify id'"
												:detailData="personifyNumber.toString()"
												:icon="'fa-hashtag'"
												:editable="false"
											></itemDetail>
										</div>
									</div>
								</li>
								<li
									class="br-b_1 br_0 br_secondary-5 br_solid p_2 p-l_0 p-b_0 p-b_2:md cell p-r_5:lg small-12 large-6 grid-x"
								>
									<itemDetail
										:label="'username'"
										:detailData="userName"
										:icon="'fa-user-circle'"
										:editable="true"
										v-on:emitBasic="function(){$emit('open-username-reveal')}"
									></itemDetail>
								</li>

								<li
									class="br-b_1 br_0 br_secondary-5 br_solid p_2 p-l_0 p-b_0 p-b_2:md cell p-r_5:lg small-12 large-6 grid-x"
								>
									<itemDetail
										:label="'email'"
										:detailData="emailAddress"
										:icon="'fa-envelope-open-text'"
										:editable="true"
										v-on:emitBasic="function(){$emit('open-email-reveal')}"
									></itemDetail>
								</li>
								<li
									class="br-b_1 br_0 br_secondary-5 br_solid p_2 p-l_0 p-b_0 p-b_2:md cell p-r_5:lg small-12 large-6 grid-x"
								>
									<itemDetail
										:label="'password'"
										:detailData="password"
										:icon="'fa-key'"
										:editable="true"
										v-on:emitBasic="function(){$emit('open-password-reveal')}"
										:mask="true"
									></itemDetail>
								</li>

								<li
									class="br-b_1 br_0 br_secondary-5 br_solid p_2 p-l_0 p-b_0 p-b_2:md cell p-r_5:lg small-12 large-6"
								>
									<itemDetail
										:label="'address'"
										:detailData="location"
										:icon="'fa-map-pin'"
										:editable="false"
									></itemDetail>
								</li>
								<li
									v-if="hasBadge"
									class="br-b_1 br_0 br_secondary-5 br_solid p_2 p-l_0 p-b_0 p-b_2:md cell p-r_5:lg small-12 large-6"
								>
									<itemDetail
										:label="'badge'"
										:detailData="badgeNumber.toString()"
										:icon="'fa-badge-check'"
										:editable="true"
										v-on:emitBasic="function(){$emit('open-badge-reveal')}"
									></itemDetail>
								</li>
								<li
									v-if="accountLocked"
									class="br-b_1 br_0 br_secondary-5 br_solid p_2 p-l_0 p-b_0 p-b_2:md cell p-r_5:lg m-l_0 small-12 large-6"
								>
									<itemDetail
										:label="'locked accout'"
										:detailData="'Too Many Failed Attemtps'"
										:icon="'fa-lock-alt'"
										:editable="true"
										v-on:emitBasic="function(){$emit('open-account-locked-reveal')}"
									></itemDetail>
								</li>
							</ul>
						</div>
						<div class="p_4 cell small-12 large-6 flex font_n1">
							<i class="m-l_3 c_secondary-2 fal fa-users-crown flex-shrink"></i>
							<a
								class="link m-l_3 m-r_3 flex-auto"
								v-if="children != undefined"
								@click="showChildren_toggle"
							>
								<i v-if="showChildren" class="fas fa-chevron-circle-down p-r_3"></i>
								<i v-if="!showChildren" class="fas fa-chevron-circle-r p-r_3"></i>
								{{children.length}} linked account(s)
							</a>
							<a @click="selectable_toggle" class="link float-right m-l_3 m-r_3 flex-1">
								manage &nbsp;
								<i class="fal fa-pencil"></i>
							</a>
						</div>
					</div>
					<div
						class="cell shrink texture_light bg_secondary-3 m-r_3 c_secondary-1 font_n2 font_n1:md font_0:lg"
						v-bind:class="{ 'bg_warning-n3': selectable }"
					>
						<ul class="flag no-bullet flex flex-dir-column">
							<li class="p_3 center member" v-bind:class="{ active: hasMembership }">
								<i class="fal fa-user-md"></i>
							</li>
							<li class="p_3 center accbadge" v-bind:class="{ active: hasBadge }">
								<i class="fal fa-badge-check"></i>
							</li>
							<li class="p_3 center iscience" v-bind:class="{ active: hasIScience }">
								<i class="fal fa-flask"></i>
							</li>
							<li
								v-if="hasaccountLocked"
								class="p_3 center account-locked active"
								@click="$emit('open-account-locked-reveal');"
							>
								<i class="fal fa-lock-alt"></i>
							</li>
							<li v-else class="p_3 center account-locked">
								<i class="fal fa-lock-alt"></i>
							</li>
							<li class="p_3 center expobadge" v-bind:class="{ active: hasExpoBadge }">
								<i class="fal fa-exclamation-circle"></i>
							</li>
						</ul>
					</div>
				</div>
			</div>
		</div>

		<transition-group name="slideInDown" tag="div">
			<linkedResult
				v-for="(child, index) in children"
				v-if="showChildren"
				v-bind="child"
				v-on:breaklink="$emit('breakLinkRepeater',{child:child})"
				v-bind:key="index"
			/>
		</transition-group>
	</div>
</template>
<script>
import linkedResult from "@/components/linkedResult.vue";
import itemDetail from "@/components/subComponents/itemDetail.vue";

export default {
	name: "searchResults",
	components: {
		linkedResult,
		itemDetail
	},
	props: {
		member: Object,
		fullName: String,
		personifyNumber: Number,
		badgeNumber: { type: Number, default: -1 },
		emailAddress: { type: String, default: "" },
		location: String,
		userName: String,
		password: String,
		memberType: { type: String, default: "NPI" },
		iscienceBadge: { type: Number, default: -1 },
		accountLocked: { type: Boolean, default: false },
		expoBadge: { type: Boolean, default: false },
		children: { type: Array, default: undefined }
	},
	computed: {
		memberIconComputed: function() {
			return this.hasMembership ? "fa-user-md" : "fa-user-slash";
		},
		hasMembership: function() {
			if (this.memberType != "" && this.memberType != "NPI") {
				return true;
			}
			return false;
		},
		hasBadge: function() {
			return this.badgeNumber == -1 ? false : true;
		},
		hasIScience: function() {
			return this.iscienceBadge == -1 ? false : true;
		},
		hasaccountLocked: function() {
			return this.accountLocked;
		},
		hasExpoBadge: function() {
			return this.expoBadge;
		},
		memberTypeComputed: function() {
			if (this.memberType == "") {
				return "NPI";
			} else {
				return this.memberType;
			}
		}
	},
	data() {
		return {
			selectable: false,
			selected: true,
			showChildren: false
		};
	},
	methods: {
		set_selectable: function($set) {
			this.selectable = $set;
		},
		selectable_toggle: function() {
			this.selectable = !this.selectable;
		},
		showChildren_toggle: function() {
			this.showChildren = !this.showChildren;
		},
		select_toggle: function() {
			if (this.selectable) {
				this.selected = !this.selected;
			}
		}
	}
};
</script>
<style lang="scss">
.turn90 {
	transform: rotate(90deg);
}
.slideIn-enter-active,
.slideIn-leave-active {
	transition: transform 0.5s ease, opacity 0.25s ease 0.25s;
}
.slideIn-enter, .slideIn-leave-to /* .slideIn-leave-active below version 2.1.8 */ {
	transform: translateX(-100%);
	opacity: 0;
}
.slideIn-enter-to {
	transform: translateX(1);
	opacity: 1;
}
.slideInDown-enter-active,
.slideInDown-leave-active {
	transition: transform 0.5s ease, opacity 0.25s ease 0.25s;
}
.slideInDown-enter, .slideInDown-leave-to /* .slideIn-leave-active below version 2.1.8 */ {
	transform: translateY(-100%);
	opacity: 0;
}
.slideInDown-enter-to {
	transform: translateY(1);
	opacity: 1;
}
.slideInDown-move {
	transition: transform 0.5s ease;
}

.result {
	display: flex;
	flex-direction: row;
	position: relative;
}

.searchResults {
	z-index: 100;
}

.select-container {
	flex: 0 0 2.25rem;
	padding: 0;
	text-align: center;
	max-width: 0;
	transition: max-width 0.5s;
}

.selectable .select-container {
	max-width: 200px;
}

.detail-container {
	flex: 1 1 auto;
}

.flag .active {
	color: white;
}

.flag .member.active {
	background-color: #2e3192;
}

.flag .accbadge.active {
	background-color: #00a400;
}

.flag .iscience.active {
	background-color: #6233c9;
}

.flag .account-locked.active {
	background-color: #c70a0a;
}

.flag .expobadge.active {
	background-color: #db6d12;
}

.bounce-transition {
	display: inline-block;
}

.bounce-enter {
	width: 2rem;
	animation: bounce-in 0.5s;
}

.bounce-leave-to {
	animation: bounce-out 0.5s;
}

@keyframes bounce-in {
	0% {
		transform: translateX(0rem) scaleX(1);
	}

	25% {
		transform: translateX(0.25rem) scaleX(1.25);
	}

	100% {
		transform: translateX(-2rem) scaleX(0);
	}
}

@keyframes bounce-out {
	0% {
		transform: translateX(0rem) scaleX(1);
	}

	25% {
		transform: translateX(0.25rem) scaleX(1.25);
	}

	100% {
		transform: translateX(-2rem) scaleX(0);
	}
}
</style>
