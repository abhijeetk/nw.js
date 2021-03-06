# App {: .doctitle}

---

!!! important "Available"
    Since 0.3.1

[TOC]

## App.argv

!!! important "Available"
    Since 0.3.1

Get the command line arguments when starting the app.

## App.fullArgv

!!! important "Available"
    Since 0.3.1

Get all the command line arguments when starting the app. Because NW.js itself used switches like `--no-sandbox` and `--process-per-tab`, it would confuse the app when the switches were meant to be given to NW.js, so `App.argv` just filtered such switches (arguments' precedence were kept). You can get the switches to be filtered with `App.filteredArgv`.

## App.dataPath

!!! important "Available"
    Since 0.6.1

Get the application's data path in user's directory.

* Windows: `%LOCALAPPDATA%/<name>`
* Linux: `~/.config/<name>`
* OSX: `~/Library/Application Support/<name>`

`<name>` is the field in the manifest.

## App.manifest

!!! important "Available"
    Since 0.7.0

Get the JSON object of the manifest file.

## App.clearCache()

!!! important "Available"
    Since 0.6.0

Clear the HTTP cache in memory and the one on disk. This method call is synchronized.

## App.closeAllWindows()

!!! important "Available"
    Since 0.3.2

Send the `close` event to all windows of current app, if no window is blocking the `close` event, then the app will quit after all windows have done shutdown. Use this method to quit an app will give windows a chance to save data.

## App.crashBrowser()
## App.crashRenderer()

!!! important "Available"
    Since 0.8.0

These 2 functions crashes the browser process and the renderer process respectively, to test the [Crash dump](../For Developers/Understanding Crash Dump.md) feature.

## App.getProxyForURL(url)

!!! important "Available"
    Since 0.6.3

* `url` `{String}` the URL to query for proxy

Query the proxy to be used for loading `url` in DOM. The return value is in the same format used in [PAC](http://en.wikipedia.org/wiki/Proxy_auto-config) (e.g. "DIRECT", "PROXY localhost:8080").

## App.setProxyConfig(config)

!!! important "Available"
    Since 0.11.1

* `config` `{String}` Proxy rules

Set the proxy config which the web engine will be used to request network resources.

Rule (copied from [`net/proxy/proxy_config.h`](https://github.com/nwjs/chromium.src/blob/nw13/net/proxy/proxy_config.h))

```
    // Parses the rules from a string, indicating which proxies to use.
    //
    //   proxy-uri = [<proxy-scheme>"://"]<proxy-host>[":"<proxy-port>]
    //
    //   proxy-uri-list = <proxy-uri>[","<proxy-uri-list>]
    //
    //   url-scheme = "http" | "https" | "ftp" | "socks"
    //
    //   scheme-proxies = [<url-scheme>"="]<proxy-uri-list>
    //
    //   proxy-rules = scheme-proxies[";"<scheme-proxies>]
    //
    // Thus, the proxy-rules string should be a semicolon-separated list of
    // ordered proxies that apply to a particular URL scheme. Unless specified,
    // the proxy scheme for proxy-uris is assumed to be http.
    //
    // Some special cases:
    //  * If the scheme is omitted from the first proxy list, that list applies
    //    to all URL schemes and subsequent lists are ignored.
    //  * If a scheme is omitted from any proxy list after a list where a scheme
    //    has been provided, the list without a scheme is ignored.
    //  * If the url-scheme is set to 'socks', that sets a fallback list that
    //    to all otherwise unspecified url-schemes, however the default proxy-
    //    scheme for proxy urls in the 'socks' list is understood to be
    //    socks4:// if unspecified.
    //
    // For example:
    //   "http=foopy:80;ftp=foopy2"  -- use HTTP proxy "foopy:80" for http://
    //                                  URLs, and HTTP proxy "foopy2:80" for
    //                                  ftp:// URLs.
    //   "foopy:80"                  -- use HTTP proxy "foopy:80" for all URLs.
    //   "foopy:80,bar,direct://"    -- use HTTP proxy "foopy:80" for all URLs,
    //                                  failing over to "bar" if "foopy:80" is
    //                                  unavailable, and after that using no
    //                                  proxy.
    //   "socks4://foopy"            -- use SOCKS v4 proxy "foopy:1080" for all
    //                                  URLs.
    //   "http=foop,socks5://bar.com -- use HTTP proxy "foopy" for http URLs,
    //                                  and fail over to the SOCKS5 proxy
    //                                  "bar.com" if "foop" is unavailable.
    //   "http=foopy,direct://       -- use HTTP proxy "foopy" for http URLs,
    //                                  and use no proxy if "foopy" is
    //                                  unavailable.
    //   "http=foopy;socks=foopy2   --  use HTTP proxy "foopy" for http URLs,
    //                                  and use socks4://foopy2 for all other
    //                                  URLs.
```

## App.quit()

!!! important "Available"
    Since 0.3.1

Quit current app. This method will **not** send `close` event to windows and app will just quit quietly.

## App.setCrashDumpDir(dir)

!!! warning "Deprecated"
    Since 0.11.0

* `dir` `{String}` path to generate the crash dump

Set the directory where the minidump file will be saved on crash. For more information, see [Crash dump](../For Developers/Understanding Crash Dump.md).

This API was **deprecated** since 0.11.0.

## App.addOriginAccessWhitelistEntry(sourceOrigin, destinationProtocol, destinationHost, allowDestinationSubdomains)

!!! important "Available"
    Since 0.10.0

* `sourceOrigin` `{String}` The source origin. e.g. `http://github.com/`
* `destinationProtocol` `{String}` The destination protocol where the `sourceOrigin` can access to. e.g. `app`
* `destinationHost` `{String}` The destination host where the `sourceOrigin` can access to. e.g. `myapp`
* `allowDestinationSubdomains` `{Boolean}` If set to true, the `sourceOrigin` is allowed to access subdomains of destinations.

Add an entry to the whitelist used for controlling cross-origin access. Suppose you want to allow HTTP redirecting from `github.com` to the page of your app, use something like this with the [App protocol](../For Users/Advanced/App Protocol.md):

```javascript
App.addOriginAccessWhitelistEntry('http://github.com/', 'app', 'myapp', true);
```

Use `App.removeOriginAccessWhitelistEntry` with exactly the same arguments to do the contrary.

## App.removeOriginAccessWhitelistEntry(sourceOrigin, destinationProtocol, destinationHost, allowDestinationSubdomains)

!!! important "Available"
    Since 0.10.0

* `sourceOrigin` `{String}` The source origin. e.g. `http://github.com/`
* `destinationProtocol` `{String}` The destination protocol where the `sourceOrigin` can access to. e.g. `app`
* `destinationHost` `{String}` The destination host where the `sourceOrigin` can access to. e.g. `myapp`
* `allowDestinationSubdomains` `{Boolean}` If set to true, the `sourceOrigin` is allowed to access subdomains of destinations.

Remove an entry from the whitelist used for controlling cross-origin access. See `addOriginAccessWhitelistEntry` above.

## App.registerGlobalHotKey(shortcut)

!!! important "Available"
    Since 0.10.0

* `shortcut` `{Shortcut}` the `Shortcut` object to register.

Register a global keyboard shortcut (also known as system-wide hot key) to the system.

See [Shortcut](Shortcut.md) for more information.

## App.unregisterGlobalHotKey(shortcut)

!!! important "Available"
    Since 0.10.0

* `shortcut` `{Shortcut}` the `Shortcut` object to unregister.

Unregisters a global keyboard shortcut.

See [Shortcut](Shortcut.md) for more information.

## Event: open(args)

!!! important "Available"
    Since 0.3.2

* `args` `{String}` the full command line of the program.

Emitted when users opened a file with your app. For more on this, see [Handle CLI Arguments](../For Users/Advanced/Handle CLI Arguments.md).

!!! note
    Before 0.7.0, `args` is the argument in the command line and the event is sent multiple times for each of the arguments.

## Event: reopen

!!! important "Available"
    Since 0.7.3

This is a Mac specific feature. This event is sent when the user clicks the dock icon for an already running application.