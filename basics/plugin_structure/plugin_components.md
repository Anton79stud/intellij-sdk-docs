---
title: Plugin Components
---

Plugin components are a legacy feature supported for compatibility with plugins created for older versions of the
IntelliJ Platform. Plugins using components do not support dynamic loading (the ability to install, update and
uninstall plugins without restarting the IDE). When writing new plugins, you should avoid creating components,
and you should migrate existing components in your plugins to services, extensions or listeners.

Plugin components are defined using `<application-components>`, `<project-components>` and `<module-components>`
tags in a plugin.xml file. 

To migrate your code from components to more modern APIs, please use the following guidelines:

  * To manage some state or logic that is only needed when the user performs a specific operation,
    use a [Service](plugin_services.md).
  * To store the state of your plugin at the application or project level, use a [Service](plugin_services.md)
    and implement the `PersistentStateComponent` interface. See [Persisting State of Components](/basics/persisting_state_of_components.md) for details.
  * To subscribe to events, use a [listener](plugin_listeners.md) or create an [extension](plugin_extensions.md) for a dedicated extension point (for example, `<editorFactoryListener>`) if one exists for the event you need to subscribe to.
  * Executing code on application startup should be avoided whenever possible, because it slows down startup.
    Plugin code should only be executed when projects are opened or when the user invokes an action of your plugin. If you can't avoid this, create
    an [extension](plugin_extensions.md) for the `<appLifecycleListener>` extension point.
  * To execute code when a project is being opened, create an [extension](plugin_extensions.md) for the `<postStartupActivity>` or `<backgroundPostStartupActivity>` extension point (the latter is supported starting with version 2019.3 of the platform).
  * To execute code on project closing or application shutdown, implement the `Disposable` interface in a [Service](plugin_services.md)
    and place the code in the `dispose` method, or use `Disposer.register` passing a `Project` or `Application` instance
    as the `parent` argument.
