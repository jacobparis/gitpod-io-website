---
section: configure
title: Configure Ports
---

<script context="module">
  export const prerender = true;
</script>

# Configure Ports

When your project starts a service that listens on a given port, Gitpod automatically serves traffic to this port of your application on an authenticated URL.

If you want to configure ports, such as: their visibility, what Gitpod does when it detects a new port being available, etc, you do that in the `ports` section of the [`.gitpod.yml`](/docs/references/gitpod-yml) configuration file.

For example, the following configuration automatically opens a preview browser tab for the application running on port 8080.

```yaml
ports:
  - port: 8080
    onOpen: open-browser
```

Ports are mapped to their own URLs. In a workspace you can get the URL mapped to a port using `gp url <port>`. At the moment you can only expose HTTP servers.

## Port Ranges

You can configure entire ranges and tell Gitpod how it should behave when a service in such a range is opened.

For instance, the following configs will stop notifying the user when a service opens on a port between 3000 and 8999:

```yaml
ports:
  - port: 3000-8999
    onOpen: ignore
```

## Configure the Opening Behavior

When a newly started service gets detected, you usually get a notification telling you about it.
If it is not yet exposed, because it is not listed in the `.gitpod.yml`, the notification will allow you to expose the port.
If it is already exposed, either through the `.gitpod.yml` or by manually exposing it, the notification asks
whether and how to open it.

To change this behavior you can provide the property `onOpen`.
It has four possible values:

- `notify` (default) - Show the notification.
- `open-preview` - Open in the preview on the right.
- `open-browser` - Open in a browser tab.
- `ignore` - Don't do anything.

Any changes to the `.gitpod.yml` will have effect immediately.

![.gitpod.yml containing ports configuration](../../../static/images/docs/ports-onOpen.png)

## Configure port visibility

By default, all ports are in private visibility state.

To change this behavior you can provide the property `visibility`.
It has two possible values:

- `private` (default) - Only allow users with workspace access to access the port.
- `public` - Allows everyone with the port URL to access the port.

### CORS

If you start a server on a private port, let's say 5001, and want to connect to it from your web application which runs on a different port, e.g. 3000, you have to configure your requests. This is necessary because Gitpod requires credentials for private ports. Without credentials, Gitpod cannot verify that the request is made by an authorized user.

**Configure your web application**

To make this work, your web application's `fetch` request needs to have the `credentials: "include"` option set. See the [MDN doc's `credentials` description](https://developer.mozilla.org/en-US/docs/Web/API/fetch) for more details.

**Configure your server**

In your server (the one on port 5001 in the above example), you have to configure the response to include the `Access-Control-Allow-Credentials` header. Without it, your browser rejects the response and you see CORS errors in the browser console.
