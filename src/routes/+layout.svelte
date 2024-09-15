<script>
	import 'carbon-components-svelte/css/all.css';
	import {
		Header,
		HeaderNav,
		HeaderNavItem,
		HeaderNavMenu,
		SideNav,
		SideNavItems,
		SideNavMenu,
		SideNavMenuItem,
		SideNavLink,
		SideNavDivider,
		SkipToContent
	} from 'carbon-components-svelte';

	let isSideNavOpen = true;
</script>

<Header company="Open Source" platformName="Bionic Reader" bind:isSideNavOpen>
	<svelte:fragment slot="skip-to-content">
		<SkipToContent />
	</svelte:fragment>
	<HeaderNav>
		<HeaderNavItem href="/" text="Home" />
		<HeaderNavItem href="/about" text="About" />
		<HeaderNavItem href="/convert" text="Convert Text" />
	</HeaderNav>
</Header>

<SideNav bind:isOpen={isSideNavOpen}>
	<SideNavItems>
		<SideNavLink href="/" text="Home" />
		<SideNavLink href="/about" text="About" />
		<SideNavLink href="/convert" text="Convert Text" />
	</SideNavItems>
</SideNav>

<slot />
