<svelte:head>
    <title>Doc Site</title>
</svelte:head>
<script>
    import '../app.css'
    // import 'styles/main.scss'
    // import {
    //     Icon, Sidebar, Badge, SidebarItem,
    //     AppShell, AppBar, AppbarItem,
    //     ScrollView,
    //     ContextLink, ThemeSwitcher, Breadcrumbs,
    // } from 'grogui'

    // import Icon from '$components/ui/Icon.svelte'
    // import StateSpy from 'state-spy'
    // import ThemeSwitcher from '$components/ThemeSwitcher.svelte'
    import {setContext} from 'svelte'
    import * as Resizable from "$lib/components/ui/resizable";
    import { ScrollArea } from "$lib/components/ui/scroll-area/index.ts";
    import { Badge } from "$lib/components/ui/badge";
    // import { Separator } from "$lib/components/ui/separator/index.ts";
    import Icon from '$lib/documentor/icons/Icon.svelte'

    let {
        data,
        // nav,
        children,
    } = $props()

    // let theme = $state(data.locals.theme)
    let main_menu = $state({active: ''})
    setContext('main_menu', main_menu)
    setContext('icon_type', 'light')

    const root_url = typeof window !== 'undefined' ? window.location.origin : ''
    setContext('root_url', root_url)

    // const menu_size = '230px'
    // let appshell = $state()
    // $effect(() => {
    //     console.log("APPSHELL:effect", appshell.ctx.appshell_width)
    // })

    let menu_pane = $state()
    let collapsed = $state(false)

    const toggle_collapsed = () => {
        collapsed = !collapsed
        if (collapsed) {
            console.log("collapsing")
            menu_pane.collapse()
        } else {
            console.log("expanding")
            menu_pane.expand()
        }
    }
</script>

<!--<StateSpy position="top-right" data={data}/>-->
<Resizable.PaneGroup direction="horizontal" class="w-full">
    <Resizable.Pane defaultSize={15} class="relative"
        bind:pane={menu_pane}
        collapsible={true}
        collapsedSize={1.5}
        minSize={15}
        onCollapse={() => collapsed = true}
        onExpand={() => collapsed = false}>

        <button class="collapse-button" onclick={toggle_collapsed}><span>{collapsed ? '>' : '<'}</span></button>        

        <ScrollArea class="h-screen">
        <div class="flex flex-col" class:hidden={collapsed}>
            {#each Object.keys(data.categories) as category}
                <section class="p-4">
                    <h3 class="text-xl font-bold">{category}</h3>
                    {#each data.categories[category] as component}
                        <a href={component.path} title={component.description} style="padding-right:var(--p3)">
                            <Icon name={component.icon} thin />
                            <span>{component.name}</span>
                            {#if (component.is_new) }
                                <Badge>new</Badge>
                            {/if}
                        </a>
                    {/each}
                </section>
            {/each}
        
        </div>
        <!-- <Separator /> -->
        </ScrollArea>
    </Resizable.Pane>

    <Resizable.Handle withHandle>
        
    </Resizable.Handle>

    <Resizable.Pane defaultSize={75} class="relative">
        <!-- <button class="font-bold p-1 border absolute left-0" onclick={toggle_collapsed}>{collapsed ? '>' : '<'}</button> -->
        {@render children?.()}
    </Resizable.Pane>

</Resizable.PaneGroup>





<!-- <AppShell bind:this={appshell}
          banner={false}
          menu={menu_size}
          menu_collapse={870}
          sidebar_collapse={1100}
          main_header={false}
          main_footer={false}
          footer={false}
          sidebar="20vw">

    {#snippet slot_header()}
        <AppBar>
                <AppbarItem>
                    <button class="sm primary"
                            onclick={() => appshell.ctx.toggle_menu()}>
                        menu
                    </button>
                </AppbarItem>
                <AppbarItem>
                    <Breadcrumbs />
                </AppbarItem>
                <AppbarItem>
                    <button class="sm primary"
                            onclick={() => appshell.ctx.toggle_sidebar()}>
                        sidebar
                    </button>
                </AppbarItem>
                <AppbarItem end>
                    <ThemeSwitcher {theme} widget="select" />
                </AppbarItem>
        </AppBar>
    {/snippet}

    {#snippet slot_sidebar()}{/snippet}

    {#snippet slot_menu()}
    <ScrollView style="background-color: var(--surface-1);color:var(--on-surface-1);position:fixed;width:{menu_size}">

        {#snippet sticky_header()}
            <ContextLink url="/" class="active-dot">
                <div class="sidebar-home" style="background-image:url({root_url}/img/yerbo-256.png);">
                    Home
                </div>
            </ContextLink>
        {/snippet}

        <Sidebar class="container-1d gui-sidebar condensed" open>
            <SidebarItem url="/theme" title="Theme" style="padding-right:var(--p3)">
                <Icon value="palette" thin />
                <span>Themes</span>
            </SidebarItem>
            <SidebarItem url="/components" title="Theme" style="padding-right:var(--p3)">
                <Icon value="web-awesome" thin />
                <span>Components</span>
            </SidebarItem>

            {#each Object.keys(data.categories) as category}
                <h3>{category}</h3>
                {#each data.categories[category] as component}
                    <SidebarItem url={component.path} title={component.description} style="padding-right:var(--p3)">
                        <Icon value={component.icon} thin />
                        <span>{component.name}</span>
                        {#if (component.is_new) }
                            <Badge xs>new</Badge>
                        {/if}
                    </SidebarItem>
                {/each}
            {/each}
            <div class="spacer my10"></div>
        </Sidebar>
    </ScrollView>
    {/snippet}

    {#snippet slot_main()}
    <main>
        <div class="container">
            {@render children?.()}
        </div>
    </main>
    {/snippet}
</AppShell> -->

<style lang="scss">
    // @use "styles/scss/index.scss" as *;
    :root {
        box-sizing: border-box;

        // --primary: 222.2 47.4% 11.2%;
        // --primary-foreground: 210 40% 98%;
    }

    .collapse-button {
        --size: 1.1em;
        --fontsize: var(--size);
        position: absolute;
        top: 0;
        right: 0;
        display: inline-flex;
        align-items: baseline;
        justify-content: center;
        width: var(--size);
        height: var(--size);
        line-height: 1;
        font-size: var(--fontsize);
        font-weight: bold;
        z-index: 1000;
        border: 1px solid var(--bg-gray-400);
        // border-radius: 50%;
        padding: .5em;
        margin: 4px 0;
        background-color: #888;
        color: white;

        span {
            aspect-ratio: 1;
            display: inline-block;
            position: relative;
            top: -.55em;
        }
    }


    //.page :global(.gui-sidebar) {
    //    max-width: 300px;
    //    overflow: hidden;
    //}
    // .sidebar-home {
    //     background-repeat: no-repeat;
    //     background-size: cover;
    //     height:190px;

    //     // some magic numbering to get the text to align with the background...
    //     color: white;
    //     font-size: 2.25rem;
    //     line-height: 1.6;
    //     font-weight: 900;
    //     text-shadow: 0 0 5px black;
    //     text-align:center
    // }

    .container {
        margin: 0 2rem;
    }

    :global(.active-dot.active):after {
        //content: '';
        --size: 8px;
        width: var(--size);
        height: var(--size);
        //background-color: var(--primary-5);
        background-color: black;
        outline: 1px solid black;
        position: absolute;
        right: calc(var(--size) / 2);
        top: 50%;
        transform: translateY(-50%);
        border-radius: 50%;
    }

</style>
