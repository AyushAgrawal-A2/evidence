---
sidebar_position: 4
hide_table_of_contents: false
title: Themes and Layouts
description: Evidence supports customizing the look and feel of your project using CSS, and by overwriting or modifying the default layout.
---

Evidence supports customizing the look and feel of your project using CSS, and by overwriting or modifying the default layout.

## Custom Layout 

To customize your, add a `+layout.svelte` file to the root of your pages directory. You can customize the `EvidenceDefaultLayout` with the options below, or replace the contents of the file with an entirely new layout. 

```html
&lt;script&gt;
	import '@evidence-dev/tailwind/fonts.css';
	import '../app.css';
	import { EvidenceDefaultLayout } from '@evidence-dev/core-components';
	export let data;
&lt;/script&gt;

<EvidenceDefaultLayout {data}>
	<slot slot="content" />
</EvidenceDefaultLayout>

```

The `EvidenceDefaultLayout` component accepts the following properties for common customizations.

<PropListing
    name=title
    description="Project title that will replace the Evidence Logo."
    options="Any string"
    defaultValue=""
/>
<PropListing
    name=logo
    description="Link to an image which will replace the Evidence logo. This will also override any project title in the header. If the image is in your project's static directory, the link should be relative to the static directory."
    options="/logo.png"
    defaultValue=""
/>
<PropListing
    name=neverShowQueries
    description="Removes the option to show queries when the project is deployed. Has no effect in development."
    options={['true', 'false']}
    defaultValue=false
/>
<PropListing
    name=maxWidth
    description="Sets the width of the project in pixels. The default layout is about 1,280 px wide."
    options="Any number"
    defaultValue=""
/>
<PropListing
    name=fullWidth
    description="Sets the width of the project to full"
    options={['true', 'false']}
    defaultValue=false
/>
<PropListing
    name=hideSidebar
    description="Hides the sidebar navigation"
    options={['true', 'false']}
    defaultValue=false
/>
<PropListing
    name=hideHeader
    description="Hides the page header"
    options={['true', 'false']}
    defaultValue=false
/>
<PropListing
    name=hideBreadcrumbs
    description="Hides the breadcrumbs which appear at the top of the page"
    options={['true', 'false']}
    defaultValue=false
/>
<PropListing
    name=hideTOC
    description="Hides the table of contents (on-page links at top right of page)"
    options={['true', 'false']}
    defaultValue=false
/>
<PropListing
    name=builtWithEvidence
    description="Display a subtle link to the Evidence website at the bottom of the sidebar."
    options={['true', 'false']}
    defaultValue=false
/>
<PropListing
    name=algolia
    description="Object containing Algolia docsearch credentials"
    options="{`{{appId: 'xxx', apiKey: 'xxx', indexName: 'xxx'}}`}"
    defaultValue=""
/>
<PropListing
    name=githubRepo
    description="Link to a Github Repo which will appear in the header using the Github Logo"
    options=""
    defaultValue='https://github.com/evidence-dev/evidence'
/>
<PropListing
    name=xProfile
    description="Link to an X (Twitter) profile which will appear in the header using the X Logo"
    options=""
    defaultValue='https://twitter.com/evidence_dev'
/>
<PropListing
    name=slackCommunity
    description="Link to a slack community which will appear in the header using the slack Logo"
    options=""
    defaultValue='https://slack.evidence.dev'
/>

**Example custom option:**
```html
<EvidenceDefaultLayout {data} hideSidebar={true}>
	<slot slot="content" />
</EvidenceDefaultLayout>
```

## Custom Styles

Evidence uses [Tailwind CSS](https://tailwindcss.com) to style Evidence components and markdown, and you can use Tailwind to add your own styles.

To style with Tailwind you add *classes* to HTML elements. You can use any HTML element in your markdown.

For more information on using Tailwind, see the [Tailwind documentation](https://tailwindcss.com/docs).

<Alert status="info">

Tailwind removes styling from HTML elements by default, so should add your own styles to `<h1/>`, `<a/>` etc. 

</Alert>

### Using the Evidence Default Styles in Custom HTML

Adding the `markdown` class to an element will style it the same as Evidence markdown, e.g.  `<h1 class='markdown'/>`. 

### Examples

#### Customize Fonts

```markdown
This is the default text style, which is used when you write text in a markdown file.

<p class="text-red-600 italic font-serif">This red italic serif text is defined inside a HTML p (paragraph) element.</p>

<p class="font-mono text-blue-500 mt-3">This is blue text using a monospace font, and a custom top margin.</p>
```

## Base Styles

Include an `app.css` file in your project root directory to customize the base styles of your project.

_The recommended approach is to copy and edit the default css file from `[my-project]/.evidence/template/src/app.css`, also found in the [Evidence Github repo](https://github.com/evidence-dev/evidence/blob/main/sites/example-project/src/app.css)._

### What can be customized with app.css?

You can customize the default styles (font, size, color etc) of most HTML elements, by adjusting the default css for the project, e.g.

- Headers
- Body text
- Images
- Links
- Page Background
- etc

You **cannot** currently easily customize the default styles of the following (as these are not defined in the core css styles):

- Evidence Chart colors and themes (see custom chart color section below)
- Evidence Components

## Page Layout

If you include a `+layout.svelte` file in a directory, any markdown files in that directory (and its subdirectories) will use this layout file instead of the default layout file.

_The recommended approach is to copy and edit the default layout file from `[my-project]/.evidence/template/src/pages/+layout.svelte`, also found in the [Evidence Github repo](https://github.com/evidence-dev/evidence/blob/main/sites/example-project/src/pages/+layout.svelte)._

### What can be customized with +layout.svelte?

By default, Evidence includes a number of features on every page, which can be removed or customized e.g.

- Sidebar
- Header
- Breadcrumbs
- Table of Contents

You can also add your own elements to the default page layout.

## Chart Color Palette

You can use a custom color palette in your charts by using the `colorPalette` option.

For example, in a bar chart:

```markdown
<BarChart
    data={orders}
    x=date
    y=sales
    colorPalette={
        [
        '#cf0d06',
        '#eb5752',
        '#e88a87',
        '#fcdad9',
        ]
    }
/>
```

This can be done within each chart individually, or by using a script tag to create a variable that can be used within one page. For example:

```markdown
&lt;script&gt;
    let myColors = [
        '#cf0d06',
        '#eb5752',
        '#e88a87',
        '#fcdad9',
    ]
&lt;/script&gt;

<BarChart
    data={orders}
    x=date
    y=sales
    colorPalette={myColors}
/>

<LineChart
    data={inventory}
    x=date
    y=inventory
    colorPalette={myColors}
/>
```
