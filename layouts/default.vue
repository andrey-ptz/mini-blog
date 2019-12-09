<template>
	<el-container class="container">
		<el-header class="header">
			<div class="logo">
				<nuxt-link to="/">Мини блог</nuxt-link>
			</div>
			<div class="create-btn">
				<el-button 
					title="Добавить новую запись"
					icon="el-icon-plus" 
					type="primary" 
					size="small"
					@click="createNote">Добавить запись</el-button>
			</div>
		</el-header>
		<el-main>
			<nuxt />
		</el-main>
	</el-container>
</template>

<script>
	export default {
		methods: {
			createNote() {
				this.$router.push('/create')
			}
		}
	}
</script>

<style lang="sass">
.container
	max-width: 1024px
	margin: 0 auto

.header
	border-bottom: 1px solid #dadada
	display: flex
	justify-content: space-between
	align-items: center
</style>
