<template>
	<div class="search texture_dust bg_secondary_5" style="min-height:100vh;">
		<transition name="slideInDown">
			<div
				v-if="hasPageMessage"
				style="z-index=0"
				v-bind:class="{'bg_alert':pageMessageType=='error','bg_success':pageMessageType=='success','bg_info':pageMessageType=='message'}"
				class="font_0 m-b_0 p_3"
			>
				<p class="m-b_0 color_white">
					<span class="font_bold">{{pageMessageType.toUpperCase()}}:</span>
					{{pageMessage}}
				</p>
			</div>
		</transition>
		<div class="bg_white br_secondary-3 br_solid br-b-width_1 p_3 shadow_2" style="z-index:1;">
			<form @submit.prevent="fireSearch(searchTerm)">
				<div class="input-group m-b_0">
					<input
						class="input-group-field"
						type="text"
						v-model="searchTerm"
						placeholder="Search via Name or Email or Badge..."
					>
					<div class="input-group-button">
						<button type="submit" class="button">
							<i class="fas fa-search"></i>
						</button>
					</div>
				</div>
			</form>
		</div>
		<transition-group
			appear
			name="search"
			enter-active-class="animate fadeInr"
			appear-active-class="animated fadeInr"
			leave-active-class="animated fadeOutl"
			:duration="1000"
		>
			<searchResult
				v-for="(member, index) in resultsOnPage"
				v-bind="member"
				v-bind:key="index"
				v-on:open-email-reveal="openEmailReveal(member)"
				v-on:open-username-reveal="openUserNameReveal(member)"
				v-on:open-account-locked-reveal="openAccountLockedReveal(member)"
				v-on:open-badge-reveal="openBadgeReveal(member)"
				v-on:open-password-reveal="openPasswordReveal(member)"
				v-on:share-record="setSharedRecord(member)"
				v-on:breakLinkRepeater="breakLinkData(member,$event.child)"
			/>
		</transition-group>
		<pagination
			:paginator="{currentPage:currentPage, maxPages:maxPages, listSize:paginatorListSize}"
			class="m-r_3 m-l_3"
		></pagination>
		<reveal ref="emailReveal" class="small">
			<h3 slot="header" class="p-t_4">Edit Email</h3>
			<div slot="content">
				<div class>
					<div class="input-group m-b_4 font_1">
						<span class="input-group-label p-l_3 p-r_3 font_0">
							<i class="fal fa-at"></i>
						</span>
						<input
							class="input-group-field"
							type="email"
							placeholder="youremail@email.com"
							style="height:unset;"
							v-model="editEmailAddress"
						>
						<div class="input-group-button" v-if="editEmailAddress !=''">
							<button
								type="submit"
								class="button secondaryalt hollow p-l_3 p-r_3"
								@click="editEmailAddress=''"
							>
								<i class="fas fa-times"></i>
							</button>
						</div>
						<div class="input-group-button">
							<button
								type="submit"
								class="button"
								v-bind:disabled="editEmailAddress==''"
								@click="changeEmail(editEmailAddress)"
							>Save</button>
						</div>
					</div>
				</div>
				<emailPostpend v-on:email-postpend="constructEmail($event)"></emailPostpend>
				<div class>
					<div class="padding-t_2">
						<button
							class="button secondary expanded small m-b_0"
							data-close
							aria-label="Close modal"
						>Cancel</button>
					</div>
				</div>
			</div>
		</reveal>
		<reveal ref="userName">
			<h3 slot="header" class="p-t_4">Edit UserName</h3>
			<div slot="content">
				<div class>
					<div class="input-group m-b_4 font_0">
						<span class="input-group-label p-l_3 p-r_3">
							<i class="fal fa-user-circle"></i>
						</span>
						<input
							class="input-group-field font_1"
							type="email"
							style="height:unset"
							placeholder="youremail@email.com"
							v-model="editUserName"
						>
						<div class="input-group-button" v-if="editUserName !=''">
							<button
								type="submit"
								class="button secondaryalt hollow p-l_3 p-r_3"
								@click="editUserName=''"
							>
								<i class="fas fa-times"></i>
							</button>
						</div>
						<div class="input-group-button">
							<button
								type="submit"
								class="button"
								v-bind:disabled="editUserName==''"
								@click="changeUserName(editUserName)"
							>Save</button>
						</div>
					</div>
				</div>
				<emailPostpend v-on:email-postpend="constructUsername($event)"></emailPostpend>
				<div class>
					<div class="padding-t_2">
						<button
							class="button secondary expanded small m-b_0"
							data-close
							aria-label="Close modal"
						>Cancel</button>
					</div>
				</div>
			</div>
		</reveal>
		<reveal ref="badgeReveal">
			<h3 slot="header" class="p-t_4">Edit Badge</h3>
			<div slot="content">
				<div class>
					<div class="input-group m-b_4 font_0">
						<span class="input-group-label p-l_3 p-r_3">
							<i class="fal fa-badge-check"></i>
						</span>
						<input
							class="input-group-field font_1"
							type="number"
							style="height:unset"
							v-model="editBadgeNumber"
						>
						<div class="input-group-button" v-if="editBadgeNumber !=''">
							<button
								type="submit"
								class="button secondaryalt hollow p-l_3 p-r_3"
								@click="editBadgeNumber=''"
							>
								<i class="fas fa-times"></i>
							</button>
						</div>
						<div class="input-group-button">
							<button
								type="submit"
								class="button"
								v-bind:disabled="editBadgeNumber==''"
								@click="changeBadgeNumber(editBadgeNumber)"
							>Save</button>
						</div>
					</div>
				</div>
				<div class>
					<div class="padding-t_2">
						<button
							class="button secondary expanded small m-b_0"
							data-close
							aria-label="Close modal"
						>Cancel</button>
					</div>
				</div>
			</div>
		</reveal>
		<reveal ref="AcountLocked">
			<h3 slot="header">Unlock User?</h3>
			<div slot="content">
				<p class="color_primary font_2">{{memberEdit.fullName}}</p>
				<p class="font_1">Do you want to remove the brute force lock from user?</p>
				<div class="grid-x grid-center grid-m-x">
					<div class="cell auto">
						<a @click="removeAcountLockedLock()" class="button primary expanded color_whtie">Yes</a>
					</div>
					<div class="cell auto">
						<a href class="button secondary expanded hollow" data-close aria-label="Close modal">No</a>
					</div>
				</div>
			</div>
		</reveal>
		<reveal ref="password">
			<h3 slot="header">Change Password</h3>
			<div slot="content">
				<div class>
					<div class="input-group m-b_4">
						<span class="input-group-label p-l_3 p-r_3">
							<i class="fal fa-key"></i>
						</span>
						<input
							class="input-group-field"
							type="text"
							placeholder="8 characters with numbers"
							v-model="editPassword"
						>
						<div class="input-group-button" v-if="editPassword !=''">
							<button class="button secondaryalt hollow p-l_3 p-r_3" @click="editPassword=''">
								<i class="fas fa-times"></i>
							</button>
						</div>
						<div class="input-group-button">
							<button class="button secondaryalt hollow p-l_3 p-r_3" @click="generatePassword">
								<i class="fas fa-cogs"></i>
							</button>
						</div>
					</div>
				</div>

				<div class="grid-x grid-center grid-m-x">
					<div class="cell auto">
						<a
							@click="saveChangedPassword(editPassword)"
							v-bind:disabled="editPassword==''"
							class="button primary expanded color_whtie"
						>Save</a>
					</div>
					<div class="cell auto">
						<a href class="button secondary expanded hollow" data-close aria-label="Close modal">Cancel</a>
					</div>
				</div>
			</div>
		</reveal>
	</div>
</template>

<script>
import searchResult from "@/components/searchResult.vue";
import reveal from "@/components/Reveal.vue";
import pagination from "@/components/pagination.vue";
import emailPostpend from "@/components/subComponents/emailPostpend.vue";
import { mapState, mapMutations, mapActions } from "vuex";

export default {
	name: "Search",
	components: {
		searchResult,
		reveal,
		pagination,
		emailPostpend
	},
	computed: {
		...mapState([
			"search",
			"share",
			"searchReturn",
			"passwordPart1",
			"passwordPart2",
			"passwordPart3"
		]),
		maxPages: function() {
			return Math.round(this.fullResults.length / 10);
		}
	},
	methods: {
		...mapMutations([
			"ADD_SEARCH_HISTORY",
			"REPLACE_MEMBER_DATA",
			"SET_SINGLE_RESULT",
			"UNLINK_ACCOUNT"
		]),
		...mapActions(["replaceMemberData"]),
		fireSearch: function(val) {
			this.runSearch(val);
			this.saveSearchHistory(val);
		},
		constructEmail: function(postpend) {
			this.editEmailAddress = this.editEmailAddress + postpend;
		},
		constructUsername: function(postpend) {
			this.editUserName = this.editUserName + postpend;
		},
		saveSearchHistory: function(val) {
			if (this.resultsOnPage.length > 0)
				this.ADD_SEARCH_HISTORY(
					Object({
						searchTerm: val.toLowerCase(),
						resultCount: this.resultsOnPage.length
					})
				);
		},
		runSearch: function(val) {
			var val = val.toLowerCase();
			this.fullResults = this.searchReturn.filter(function(result) {
				return (
					result.fullName.toLowerCase().includes(val) ||
					result.emailAddress.toLowerCase().includes(val) ||
					String(result.personifyNumber).includes(val) ||
					String(result.badgeNumber).includes(val) ||
					result.userName.toLowerCase().includes(val)
				);
			});
			this.resultsOnPage = this.fullResults.slice(0, 10);
		},
		openEmailReveal: function(member) {
			this.setMemberEdit(member);
			this.$refs.emailReveal.openReveal();
		},
		openBadgeReveal: function(member) {
			this.setMemberEdit(member);
			this.$refs.badgeReveal.openReveal();
		},
		changeBadgeNumber: function(val) {
			this.memberEdit.badgeNumber = Number.parseInt(val);
			this.REPLACE_MEMBER_DATA(this.memberEdit);
			this.firePageMessage(
				"success",
				"Badge of " +
					this.memberEdit.badgeNumber +
					"has been saved to " +
					this.memberEdit.fullName
			);

			this.$refs.badgeReveal.closeReveal();
		},
		changeEmail: function(val) {
			this.memberEdit.emailAddress = val;
			this.REPLACE_MEMBER_DATA(this.memberEdit);
			this.firePageMessage(
				"success",
				"Email of " +
					this.memberEdit.fullName +
					"has been saved to " +
					this.memberEdit.emailAddress
			);

			this.$refs.emailReveal.closeReveal();
		},
		openUserNameReveal: function(member) {
			this.setMemberEdit(member);
			this.$refs.userName.openReveal();
		},
		openPasswordReveal: function(member) {
			this.setMemberEdit(member);
			this.$refs.password.openReveal();
		},
		generatePassword: function() {
			this.editPassword =
				this.passwordPart1[this.randomNumber(25) - 1] +
				this.passwordPart2[this.randomNumber(25) - 1] +
				this.passwordPart3[this.randomNumber(25) - 1];
		},
		randomNumber: function(val) {
			return parseInt(Math.random() * val);
		},
		changeUserName: function(val) {
			this.memberEdit.userName = val;
			this.replaceMemberData(this.memberEdit);
			this.firePageMessage(
				"success",
				"UserName of " +
					this.memberEdit.fullName +
					"has been saved" +
					this.memberEdit.userName
			);
			this.$refs.userName.closeReveal();
		},
		openAccountLockedReveal: function(member) {
			this.setMemberEdit(member);
			this.$refs.AcountLocked.openReveal();
		},
		removeAcountLockedLock: function() {
			this.memberEdit.AcountLockedLock = false;
			this.REPLACE_MEMBER_DATA(this.memberEdit);
			this.$refs.AcountLocked.closeReveal();
			this.firePageMessage(
				"success",
				"Brute force lockout has been removed from the account of " +
					this.memberEdit.fullName
			);
		},
		breakLinkData: function(parent, child) {
			let payload = Object({ parent: parent, child: child });
			this.UNLINK_ACCOUNT(payload);
		},
		saveChangedPassword: function(val) {
			this.memberEdit.password = val;
			this.REPLACE_MEMBER_DATA(this.memberEdit);
			this.$refs.password.closeReveal();
		},
		setMemberEdit: function(member) {
			this.memberEdit = member;
			this.editUserName = member.userName;
			this.editPassword = member.password;
			this.editEmailAddress = member.emailAddress;
			this.editBadgeNumber = member.badgeNumber;
		},
		setSharedRecord: function(member) {
			this.SET_SINGLE_RESULT(member);
			this.firePageMessage(
				"message",
				"Member Number " + member.personifyNumber + " has been shared."
			);
		},
		clearPageMessge: function() {
			this.hasPageMessage = false;
			this.pageMessageType = "message";
			this.pageMessage = "";
		},
		firePageMessage: function(type, text) {
			this.hasPageMessage = true;
			this.pageMessageType = type;
			this.pageMessage = text;
			setTimeout(this.clearPageMessge, 3000);
		}
	},
	data() {
		return {
			editEmailAddress: "",
			editUserName: "",
			editPassword: "",
			editBadgeNumber: "",
			memberEdit: {},
			searchTerm:
				this.$route.query.q !== undefined ? this.$route.query.q : "",
			hasPageMessage: false,
			pageMessageType: "message",
			pageMessage: "",
			currentPage: 1,
			paginatorListSize: 5,
			resultsOnPage: [],
			fullResults: []
		};
	},
	mounted() {
		this.runSearch(
			this.$route.query.q !== undefined ? this.$route.query.q : ""
		);
	}
};
</script>
<style lang="css" scoped>
.white-space_nowrap {
	white-space: nowrap;
}

.link:hover {
	text-decoration: underline;
}
.slideInDown-move {
	transition: transform 0.5s ease;
}
</style>
