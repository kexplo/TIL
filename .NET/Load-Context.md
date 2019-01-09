# .NET Assembly Load Context (or Binding Context)

## .NET Framework

### TL;DR

- the [Assembly.Load](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.assembly.load) method is loaded with Default Load Context.
- the [Assembly.LoadFrom](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.assembly.loadfrom) method is loaded with Load-From Context. It enables dependencies to be located and loaded from that path. In addition, assemblies in this context can use dependencies that are loaded into the Default Load Context.
- the [Assembly.LoadFile](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.assembly.loadfile) method is loaded without any context. so its dependencies are not automatically loaded. (You might have a handler for the AppDomain.AssemblyResolve.)

----

### Default Load Context

When assemblies are loaded into the default load context, their dependencies are loaded automatically. Dependencies that are loaded into the default load context are found automatically for assemblies in the default load context or the load-from context.

Disadventages:

- Dependencies that are loaded into other contexts are not available.
- You cannot load assemblies from locations outside the probing path into the default load context.

### Load-From Context

The load-from context lets you load an assembly from a path that is not under the application path, and therefore is not included in probing. It enables dependencies to be located and loaded from that path, because the path information is maintained by the context. In addition, assemblies in this context can use dependencies that are loaded into the default load context.

Loading assemblies by using the Assembly.LoadFrom method, or one of the other methods that load by path, has the following disadvantages:

- If an assembly with the same identity is already loaded, LoadFrom returns the loaded assembly even if a different path was specified.
- If an assembly is loaded with LoadFrom, and later an assembly in the default load context tries to load the same assembly by display name, the load attempt fails. This can occur when an assembly is deserialized.
- If an assembly is loaded with LoadFrom, and the probing path includes an assembly with the same identity but in a different location, an InvalidCastException, MissingMethodException, or other unexpected behavior can occur.
- LoadFrom demands FileIOPermissionAccess.Read and FileIOPermissionAccess.PathDiscovery, or WebPermission, on the specified path.
- If a native image exists for the assembly, it is not used.
- The assembly cannot be loaded as domain-neutral.
- In the .NET Framework versions 1.0 and 1.1, policy is not applied.

### No Context

Loading without context is the only option for transient assemblies that are generated with reflection emit. Loading without context is the only way to load multiple assemblies that have the same identity into one application domain. The cost of probing is avoided.

Disadvantages:

- Other assemblies cannot bind to assemblies that are loaded without context, unless you handle the AppDomain.AssemblyResolve event.
- Dependencies are not loaded automatically. You can preload them without context, preload them into the default load context, or load them by handling the AppDomain.AssemblyResolve event.
- Loading multiple assemblies with the same identity without context can cause type identity problems similar to those caused by loading assemblies with the same identity into multiple contexts. See Avoid Loading an Assembly into Multiple Contexts.
- If a native image exists for the assembly, it is not used.
- The assembly cannot be loaded as domain-neutral.
- In the .NET Framework versions 1.0 and 1.1, policy is not applied.

----

reference: https://docs.microsoft.com/en-us/dotnet/framework/deployment/best-practices-for-assembly-loading

## .NET Core

**LoadContext** can be viewed as a container for assemblies, their code and data (e.g. statics). Whenever an assembly is loaded, it is loaded within a load context - independent of whether the load was triggered explicitly (e.g. via Assembly.Load), implicitly (e.g. resolving static assembly references from the manifest) or dynamically (by emitting code on the fly).

In .NET Core, we have exposed a [managed API surface](https://github.com/dotnet/corefx/blob/master/src/System.Runtime.Loader/ref/System.Runtime.Loader.cs) that developers can use to interact with it - to inspect loaded assemblies or create their own **LoadContext** instance. Here are some of the scenarios that motivated this work:

Here are some of the scenarios that motivated this work:

- Ability to load multiple versions of the same assembly within a given process (e.g. for plugin frameworks)
- Ability to load assemblies explicitly in a context isolated from that of the application.
- Ability to override assemblies being resolved from application context.
- Ability to have isolation of statics (as they are tied to the **LoadContext**)
- Expose LoadContext as a first class concept for developers to interface with and not be a magic.

### Default LoadContext

Every .NET Core app has a **LoadContext** instance created during .NET Core Runtime startup that we will refer to as the *Default LoadContext*. All application assemblies (including their transitive closure) are loaded within this **LoadContext** instance.

### Custom LoadContext

For scenarios that wish to have isolation between loaded assemblies, applications can create their own **LoadContext** instance by deriving from **System.Runtime.Loader.AssemblyLoadContext** type and loading the assemblies within that instance.

Multiple assemblies with the same simple name cannot be loaded into a single load context *(Default or Custom)*. Also, .Net Core ignores strong name token for assembly binding process.

### API Surface

Most of the **AssemblyLoadContext** API surface is self-explanatory.

#### Default

This property will return a reference to the *Default LoadContext*.

#### Load

This method should be overriden in a *Custom LoadContext* if the intent is to override the assembly resolution that would be done during fallback to *Defaut LoadContext*

#### LoadFromAssemblyName

This method can be used to load an assembly into a load context different from the load context of the currently executing assembly. The assembly will be loaded into the load context on which the method is called. If the context can't resolve the assembly in its **Load** method the assembly loading will defer to the **Default** load context. In such case it's possible the loaded assembly is from the **Default** context even though the method was called on a non-default context.

Calling this method directly on the AssemblyLoadContext.Default will only load the assembly from the Default context. Depending on the caller the Default may or may not be different from the load context of the currently executing assembly.

To make sure a specified assembly is loaded into the specified load context call **AssemblyLoadContext.LoadFromAssemblyPath** and specify the path to the assembly file.

### Assembly Load APIs and LoadContext

- Assembly.Load - loads the assembly into the context of the assembly that triggers the load.
- Assembly.LoadFrom - loads the assembly into the *Default LoadContext*
- Assembly.LoadFile - creates a new (anonymous) load context to load the assembly into.
- Assembly.Load(byte[]) - creates a new (anonymous) load context to load the assembly into.

references:

- https://github.com/dotnet/coreclr/blob/master/Documentation/design-docs/assemblyloadcontext.md
- https://github.com/dotnet/corefx/tree/master/src/System.Runtime.Loader