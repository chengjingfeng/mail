<template>
	<Content app-name="mail">
		<Navigation />
		<AppContent>
			<div class="section">
				<h2>{{ t('mail', 'Account Settings') }} - {{ email }}</h2>
				<h3>{{ t('mail', 'Mail server') }}</h3>
				<div id="mail-settings">
					<AccountForm :display-name="displayName" :email="email" :save="onSave" :account="account" />
				</div>
				<SignatureSettings :account="account" />
			</div>
		</AppContent>
	</Content>
</template>

<script>
import {AppContent, Content} from 'nextcloud-vue'

import AccountForm from '../components/AccountForm'
import Navigation from '../components/Navigation'
import SignatureSettings from '../components/SignatureSettings'

export default {
	name: 'AccountSettings',
	components: {
		AccountForm,
		AppContent,
		Content,
		Navigation,
		SignatureSettings,
	},
	data() {
		const account = this.$store.getters.getAccount(this.$route.params.accountId)
		return {
			account,
			signature: account.signature,
		}
	},
	computed: {
		menu() {
			return this.buildMenu()
		},
		displayName() {
			return this.$store.getters.getAccount(this.$route.params.accountId).name
		},
		email() {
			return this.$store.getters.getAccount(this.$route.params.accountId).emailAddress
		},
	},
	methods: {
		onSave(data) {
			console.log('data to save:', data)
			return this.$store
				.dispatch('updateAccount', {
					...data,
					accountId: this.$route.params.accountId,
				})
				.then(account => account)
				.catch(err => {
					console.error('account update failed:', err)

					throw err
				})
		},
	},
}
</script>
