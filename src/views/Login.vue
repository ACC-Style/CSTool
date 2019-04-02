<template>
	<div class="home texture_light p_4" style="height:100vh;">
		<div class="grid-x">
			<div class="cell small-12 medium-10 large-8 m_auto" style="max-width:54em">
				<div @click="onModeChange('login')">
					<h1 class="logo-lockup p-b_3">
						<img src="assets/img/mobileLogo.svg" alt="American College of Cardiology" class="logo p-r_3">
						<span class="text">
							Customer &nbsp;
							<br class="show-for-medium">Service Tool
						</span>
					</h1>
				</div>
				<div class="card shadow_2 cell">
					<div
						class="card-divider c_white font_3 p_4 texture_dust font_bold"
						style="z-index:10"
						v-bind:class="{'bg_alert': pageHasError() , 'bg_primary': !pageHasError()}"
						@click="pageValidation()"
					>
						<transition name="crossFade" mode="out-in">
							<span v-if=" mode == 'login'">Login</span>
							<span v-if="mode == 'resetPassword_start'">Password Reset</span>
							<span v-if="mode == 'resetUsername_start'">Username Reset</span>
						</transition>
					</div>
					<div class="card-section relative">
						<transition name="slideInDown">
							<div v-if="pageHasError()" style="z-index:0" class="bg_alert-4 font_0 m-b_0 m_n4 p_3">
								<p class="m-b_0">
									<span class="font_bold">Error Loging In:</span>
									{{pageError}}
								</p>
							</div>
						</transition>
						<transition name="slideInRight" mode="out-in">
							<div v-if=" mode == 'login' " key="login">
								<form class="grid-container clearfix">
									<div class="grid-x grid-padding-x p-y_3">
										<div class="medium-6 cell">
											<inputUsername
												class="m-t_2 m-t_0:md"
												:label="'Username'"
												:required="true"
												:pageHasError="pageHasError()"
												:value="username"
												:hint="''"
												v-on:update:username="username = $event"
											></inputUsername>
											<div class="grid-x font_n2 m-t_2">
												<div class="switch tiny radius cell shrink">
													<input
														id="tinySwitch_rememberMe"
														type="checkbox"
														name="exampleSwitch"
														class="switch-input"
													>
													<label for="tinySwitch_rememberMe" class="bg_secondary-3 switch-paddle">
														<span class="show-for-sr">Remember Me</span>
													</label>
												</div>
												<label
													for="tinySwitch_rememberMe"
													class="switch-value cell auto p-l_3 c_secondary-n1 font-regular"
												>Remember Me</label>
											</div>
										</div>
										<div class="medium-6 cell">
											<inputPassword
												class="m-t_4 m-t_0:md"
												:label="'Password'"
												:required="true"
												:pageHasError="pageHasError()"
												:value="password"
												:hint="''"
												v-on:update:password="password = $event"
											></inputPassword>
											<div
												class="h:underline float-right m-t_3 m-t_0:md c_secondary-n1 h:c_primary"
												@click="onModeChange('resetPassword_start')"
											>forgot password</div>
										</div>
									</div>
								</form>
								<div class="grid-container clearfix">
									<div class="grid-x justify-end">
										<div class="medium-4 cell m-t_4 m-t_0:md">
											<a
												@click="pageValidation()"
												v-if="!submitDisabled()"
												class="button display-block c_white m-b_0 br_radius expanded disabled"
											>
												Log In
												<i class="fal fa-ban"></i>
											</a>
											<router-link
												to="/search"
												v-if="submitDisabled()"
												class="button display-block c_white m-b_0 br_radius expanded"
											>
												Log In
												<i class="fal fa-arrow-alt-right"></i>
											</router-link>
										</div>
									</div>
								</div>
							</div>
							<div v-if="mode == 'resetPassword_start'" key="start">
								<form class="grid-container m_auto" style="max-width:35em;">
									<div class="grid-x">
										<div class="cell auto">
											<p class="font_1">Select the most convient way to reset your password</p>
											<ul class="no-bullet">
												<li>
													<a
														href
														class="button secondary expanded br_radius hollow h:bg_secondary-4"
													>Text me an unlock code</a>
												</li>
												<li>
													<a
														href
														class="button secondary expanded br_radius hollow h:bg_secondary-4"
													>Email me a reset link</a>
												</li>
												<li>
													<a
														href
														class="button secondary expanded br_radius hollow h:bg_secondary-4"
													>Answer security questions</a>
												</li>
												<li
													@click="onModeChange('login')"
													class="text-center h:bg_secondary-5 font_0 underline h:none c_primary center p-x_5 p-y_2 br_radius"
												>go back to login</li>
											</ul>
										</div>
									</div>
								</form>
							</div>
							<div v-if="mode == 'resetUsername_start'" key="start">
								<form class="grid-container m_auto" style="max-width:35em;">
									<div class="grid-x">
										<div class="cell small_12">
											<p class="font_1">Enter your email address and we will email you your username.</p>
										</div>
										<div class="cell small_12">
											<inputUsername
												class="m-t_2 m-t_0:md"
												:label="'Email'"
												:required="true"
												:pageHasError="pageHasError()"
												:value="usernameResetEmail"
												:hint="''"
												v-on:update:username="username = $event"
											></inputUsername>
											<div class="grid-x grid-margin-x">
												<div @click="onModeChange('login')" class="medium-8 cell m-t_4 m-t_2:md">
													<a class="button secondary m-b_0 br_radius expanded br_radius">
														Log In
														<i class="fal fa-ban"></i>
													</a>
												</div>
												<div class="medium-4 cell m-t_4 m-t_2:md">
													<a class="button display-block c_white m-b_0 br_radius expanded disabled">
														Log In
														<i class="fal fa-ban"></i>
													</a>
													<!---->
												</div>
											</div>
										</div>
									</div>
								</form>
							</div>
						</transition>
					</div>
					<div class="texture_light p_3 text-center">
						<a class="link secondary h:underline">I don't have an account</a>
					</div>
				</div>
				<div class="p_3 font_n1 text-center">
					<router-link class="link secondary underline" to="/about">
						<i class="fas fa-info-square"></i> About this utility
					</router-link>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
// @ is an alias to /src
import inputPassword from "@/components/inputPassword.vue";
import inputUsername from "@/components/inputUsername.vue";

export default {
	name: "login",
	components: {
		inputPassword,
		inputUsername
	},
	data() {
		return {
			username: "",
			password: "",
			usernameResetEmail: "",
			pageError: "",
			mode: "login"
		};
	},
	methods: {
		submitDisabled: function() {
			if (this.username != "" && this.password != "") {
				return true;
			}
			return false;
		},
		pageHasError: function() {
			if (this.pageError != "") {
				return true;
			}
			return false;
		},
		pageValidation() {
			if (this.pageError == "") {
				this.pageError = "Your email and password does not match our records.";
			} else {
				this.pageError = "";
			}
		},
		onModeChange(value) {
			this.pageError = "";
			this.mode = value;
		}
	}
};
</script>

<style lang="scss" scoped>
.logo-lockup {
	display: flex;
	flex-direction: column;
	@media only screen and (min-width: 500px) {
		flex-direction: row;
	}
}

.logo-lockup .logo {
	height: 40px;
	flex: 0 1 0px;
	margin-bottom: 0.5rem;
	@media only screen and (min-width: 500px) {
		height: 80px;
	}
}

.logo-lockup .text {
	flex: 1 1 auto;
	line-height: 1;
	font-size: 24px;
	align-self: center;
	border-top: 3px solid #00386b;
	padding-top: 1rem;
	text-align: center;
	color: #00386b;
	@media only screen and (min-width: 500px) {
		border-left: 3px solid #00386b;
		padding-left: 1rem;
		text-align: left;
		padding-top: 0;
		border-top: 0 none;
		font-size: 40px;
	}
}

.slideInRight-enter-active,
.slideInRight-leave-active {
	transition: transform 0.5s ease, opacity 0.25s ease 0.25s;
}
/*.slideInRight-enter-active {
	transition-delay: 0.5s;
}*/
.slideInRight-leave-to /* .slideIn-leave-active below version 2.1.8 */ {
	transform: translateX(-100%);
	opacity: 0;
}
.slideInRight-enter /* .slideIn-leave-active below version 2.1.8 */ {
	transform: translateX(100%);
	opacity: 0;
}
.slideInRight-enter-to {
	transform: translateX(1);
	opacity: 1;
}
.slideInRight-move {
	transition: transform 0.5s ease;
	position: absolute;
	top: 0;
	right: 0;
	left: 0;
}

.crossFade-enter-active,
.crossFade-leave-active {
	transition: opacity 0.25s ease 0.25s;
}

.crossFade-leave-to /* .slideIn-leave-active below version 2.1.8 */ {
	opacity: 0;
}
.crossFade-enter /* .slideIn-leave-active below version 2.1.8 */ {
	opacity: 0;
}
.crossFade-enter-to {
	opacity: 1;
}
.crossFade-move {
	transition: opacity 0.5s ease;
}
</style>
