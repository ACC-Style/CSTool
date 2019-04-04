<template>
	<div class="Shared p_4">
		<div class="grid-x">
			<ul
				class="tabs texture_medium shadow_n1 cell large auto br_radius"
				data-active-collapse="true"
				data-tabs
				id="collapsing-tabs"
			>
				<li
					class="tabs-title br_radius m_2"
					v-bind:class="{'is-active':activeContent == 'reprintMembers'}"
				>
					<a
						class="font_1 c_primary-n5 h:white-3 br_radius"
						v-bind:class="{'c_primary-5 bg_primary h:bg_primary-n1':activeContent == 'reprintMembers'}"
						@click="function(){activeContent = 'reprintMembers'}"
					>Reprint</a>
				</li>
				<li
					class="tabs-title m_2 br_radius"
					v-bind:class="{'is-active':activeContent == 'sharedMembers'}"
				>
					<a
						@click="function(){activeContent = 'sharedMembers'}"
						v-bind:class="{'c_primary-5 bg_primary h:bg_primary-n1':activeContent == 'sharedMembers'}"
						class="font_1 c_primary-n5 h:white-3 br_radius"
					>Shared</a>
				</li>
				<li
					class="tabs-title m_2 br_radius"
					v-bind:class="{'is-active':activeContent == 'flaggedMembers'}"
				>
					<a
						@click="function(){activeContent = 'flaggedMembers'}"
						v-bind:class="{'c_primary-5 bg_primary h:bg_primary-n1':activeContent == 'flaggedMembers'}"
						class="font_1 c_primary-n5 h:white-3 br_radius"
					>Flagged</a>
				</li>
			</ul>
		</div>

		<div class="tabs-content bg_white-0" data-tabs-content="collapsing-tabs">
			<div
				class="tabs-panel"
				v-bind:class="{'is-active': activeContent == 'reprintMembers'}"
				id="reprintMembers"
			>
				<h2 class="br_0 br-b_1 br_solid br_primary">Reprint Badges</h2>
				<div
					v-if="shares.length == 0 || shares == undefined"
					class="m-t_4 p_4 texture_light bg_info-2 shadow_n3 br_radius"
				>You Currently have no shared member accounts.</div>
				<div
					v-for="(share , index) in shares"
					:key="index"
					class="card shadow_1 br_radius m-b_3 br_secondary-4"
				>
					<div class="card-section">
						<div class="detail-container color_black">
							<div
								class="d_block:md d_none font_n1 c_primary-n2 font_bold p-l_2 m-t_n4 grid-condtainer br-b_1 br_secondary-3 br_solid"
							>
								<h2
									class="member-name font_3 font_slab line-height_2 member-name p_3 p-l_4 p-r_3 color_ACC"
								>
									{{share.memberName}}
									<span
										class="font_0 c_white float-right bg_secondary-3 m-t_2 br_radius m-r_n3 d_inline-block p_2 p-x_3"
										v-if="share.memberType != '' && share.memberType != undefined && share.memberType != NPI"
									>
										<i class="m-l_3 m-r_3 fas fa-user-md"></i>
										{{share.memberType}}
									</span>
								</h2>
							</div>
							<ul class="details no-bullet p-x_4 m-b_3 grid-x">
								<li class="br-b_1 br_0 br_secondary-5 br_solid p_2 large-4 cell">
									<itemDetail
										:icon="'fa-hashtag'"
										:label="'personify'"
										:detailData="share.personifyNumber"
										:editable="false"
										:mask="false"
									></itemDetail>
								</li>
								<li class="br-b_1 br_0 br_secondary-5 br_solid p_2 large-4 cell">
									<itemDetail
										:icon="'fa-badge-check'"
										:label="'badge id'"
										:detailData="share.badgeNumber"
										:editable="false"
										:mask="false"
									></itemDetail>
								</li>
								<li class="br-b_1 br_0 br_secondary-5 br_solid p_2 overflow-hidden ellipsis large-4 cell">
									<itemDetail
										:icon="'fa-at'"
										:label="'email'"
										:detailData="share.emailAddress"
										:editable="false"
										:mask="false"
									></itemDetail>
								</li>
							</ul>
						</div>
						<div class="grid-x grid-margin-x">
							<div class="cell auto">
								<a
									class="button small secondary m-b_3 br_radius expanded"
									@click="openMemberRecord(share.personifyNumber)"
								>
									Go To Details
									<i class="m-l_3 m-r_3 fas fa-arrow-square-right"></i>
								</a>
							</div>
							<div class="cell auto">
								<a
									class="button small primary m-b_3 br_radius expanded"
									@click="openMemberRecord(share.personifyNumber)"
								>
									Badge Reprinted
									<i class="m-l_3 m-r_3 fas fa-print"></i>
								</a>
							</div>
						</div>
					</div>
					<div
						class="card-divider text-center font_light font_n2 c_secondary-n1 line-height_1 member-name p-l_4 p-r_4 p_2"
					>
						<span class="color_info font_bold">Shared By: {{share.sharedBy}}&nbsp;</span>

						<span class="font_normal">
							<i class="fal fa-clock"></i>
							{{share.timestamp}}
						</span>
					</div>
				</div>
			</div>
		</div>
		<div
			class="tabs-panel"
			v-bind:class="{'is-active': activeContent == 'sharedMembers'}"
			id="sharedMembers"
		>
			<h2 class="br_0 br-b_1 br_solid br_primary">Shared Members</h2>
		</div>
		<div
			class="tabs-panel"
			v-bind:class="{'is-active': activeContent == 'flaggedMembers'}"
			id="flaggedMembers"
		>
			<h2 class="br_0 br-b_1 br_solid br_primary">Flagged Members</h2>
		</div>
	</div>
</template>

<script>
import { mapState, mapMutations } from "vuex";
import itemDetail from "@/components/subComponents/itemDetail.vue";
import router from "@/router";

export default {
	name: "Shared",
	components: {
		itemDetail
	},
	data() {
		return {
			activeContent: "reprintMembers"
		};
	},
	computed: {
		...mapState(["shares", "sharesNew"])
	},
	methods: {
		...mapMutations(["SHARE_NEW_OFF"]),
		openMemberRecord: function(val) {
			router.push({ path: "search/", query: { q: val } });
		}
	},
	mounted() {
		this.$emit("share-new-off");
	}
};
</script>

<style scoped>
</style>
