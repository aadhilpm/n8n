<template>
	<div>
		<div :class="{'main-header': true, expanded: !sidebarMenuCollapsed}">
			<div class="top-menu">
				<ExecutionDetails v-if="isExecutionPage" />
				<WorkflowDetails v-else />
			</div>
		</div>
	</div>
</template>

<script lang="ts">
import mixins from 'vue-typed-mixins';
import { mapGetters } from 'vuex';

import { pushConnection } from '@/components/mixins/pushConnection';

import WorkflowDetails from '@/components/MainHeader/WorkflowDetails.vue';
import ExecutionDetails from '@/components/MainHeader/ExecutionDetails/ExecutionDetails.vue';

export default mixins(
	pushConnection,
)
	.extend({
		name: 'MainHeader',
		components: {
			WorkflowDetails,
			ExecutionDetails,
		},
		computed: {
			...mapGetters('ui', [
				'sidebarMenuCollapsed',
			]),
			isExecutionPage (): boolean {
				return ['ExecutionById'].includes(this.$route.name as string);
			},
		},
		async mounted() {
			// Initialize the push connection
			this.pushConnect();
		},
		beforeDestroy() {
			this.pushDisconnect();
		},
	});
</script>

<style lang="scss">
.el-menu--horizontal>.el-menu-item,
.el-menu--horizontal>.el-submenu .el-submenu__title,
.el-menu-item {
	height: 65px;
	line-height: 65px;
}

.el-submenu .el-submenu__title,
.el-menu--horizontal>.el-menu-item,
.el-menu.el-menu--horizontal {
	border: none !important;
}
.el-menu--popup-bottom-start {
	margin-top: 0px;
	border-top: 1px solid #464646;
	border-radius: 0 0 2px 2px;
}

.main-header {
	position: fixed;
	top: 0;
	background-color: #fff;
	height: 65px;
	width: 100%;
	box-sizing: border-box;

	padding-left: $--sidebar-width;

	&.expanded {
		padding-left: $--sidebar-expanded-width;
	}
}

.top-menu {
	display: flex;
	align-items: center;
	font-size: 0.9em;
	height: $--header-height;
	font-weight: 400;
	padding: 0 20px;
}
</style>
