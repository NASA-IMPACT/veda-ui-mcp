# VEDA UI MCP

Tools over the VEDA STAC catalog whose results show VEDA's map and catalog components inline in a chat, so a person can find and look at Earth data by talking to an AI assistant.

One meaning per word. When two words exist for the same thing, the first wins and the rest go under _Avoid_.

## Catalog

**Catalog**:
The VEDA STAC API this server reads: every Collection and its Items.
_Avoid_: database, index, STAC (the standard, not the thing)

**Collection**:
One dataset in the Catalog, such as monthly NO₂ or land cover, with its own time and space coverage. "Dataset" is fine in text the user or the model reads (tool descriptions, messages).
_Avoid_: layer, product

**Item**:
One dated scene inside a Collection.
_Avoid_: granule, file, asset

**Render**:
A Collection's named recipe for drawing its Items as a raster layer: which asset, bands, value range and colormap.
_Avoid_: style, renders metadata

**Dashboard Render**:
The Render named `dashboard`; the map uses it whenever a Collection has one.
_Avoid_: default render

**Mappable**:
Said of a Collection whose Render the Raster API can actually draw from the Collection's Items.
_Avoid_: renderable, valid, supported

**Raster API**:
The tile service that draws Items using a Render.
_Avoid_: titiler (the software), tile server

## Chat flow

**Host**:
A chat application that calls the tools and runs Widgets: Claude Desktop, claude.ai, ChatGPT, or a test host.
_Avoid_: client, frontend

**View**:
One step of the chat flow, shown inline with a tool result: the Picker, a Map View or a Compare View.
_Avoid_: screen, page, panel

**Widget**:
A View running inside the Host, in a sandboxed frame next to the chat.
_Avoid_: app, embed, iframe (the mechanism)

**Picker**:
The View that lists Collections as cards to choose from.
_Avoid_: collection list, selector, search results

**Pick**:
A person choosing a Collection in the Picker; it reaches the model as a message from that person, and the model decides the next step.
_Avoid_: selection, click

**Map View**:
The View showing one Collection's raster layer over a date range.
_Avoid_: single-layer map, map widget

**Compare View**:
The View showing two raster layers with a swipe divider: one Collection at two date ranges, or two Collections.
_Avoid_: split view, diff map, before/after (one use of it)

**Corrective error**:
A tool failure worded so the model can fix its own call, naming what to do instead.
_Avoid_: validation error, user error

## Delivery

**View Contract**:
The agreed shape of every View as it travels from the server to the Widget, shared by both sides.
_Avoid_: payload, schema, DTO

**Connector**:
The URL a Host adds to reach this server over the internet.
_Avoid_: integration, plugin, endpoint (in text addressed to the user)

**Path Token**:
The secret segment of the Connector URL; holding the URL is the only permission needed.
_Avoid_: API key, password, auth token
