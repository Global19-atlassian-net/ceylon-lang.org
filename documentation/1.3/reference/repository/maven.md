---
layout: reference13
title_md: Maven repositories
tab: documentation
unique_id: docspage
author: Stéphane Épardaud
doc_root: ../..
---

# #{page.title_md}

Ceylon's module system integrates with Maven repositories, and can obtain
dependencies (Java `jar` archives) from a Maven repository via Aether.

## Importing Maven modules

Maven modules are seamlessly supported in the Ceylon module descriptor,
but:

- the repository type `maven:` should be explicitly specified,
- the `:` separator must be used to separate Maven group and artifact ids, 
  and
- the artifact id must be quoted.

For example, this line, occurring in `module.ceylon`, specifies a 
dependency on Hibernate ORM:

<!-- try: -->
    import maven:org.hibernate:"hibernate-core" "5.0.4.Final";

Note that, from the point of view of your Ceylon code, the name of the
imported module is the whole string `org.hibernate:hibernate-core`. However, 
the packages belonging to this module are named simply `org.hibernate`, 
`org.hibernate.boot`, etc.

### Maven group and artifact ids 

Ceylon uses a single identifier for module names, but Maven uses:

- a _group id_, which usually obeys the Java package name format, 
- together with an _artifact id_, which identifies a `.jar` file, and is
  usually of form `foo-bar-baz`.

So to import the Maven module with group id `org.hibernate` and artifact id 
`hibernate-core`, we formed a module name by first quoting the artifact id, 
and then concatenating the two identifiers with a `:` (colon), resulting 
in the module name `org.hibernate:"hibernate-core"` seen above.

### Specifying explicit Maven settings 

If you have special requirements&mdash;for example, if you need to specify an 
additional Maven repository&mdash;and need a Maven [`settings.xml`][], you can 
specify the file using the `--rep` flag:

<!--lang: none -->
    ceylon compile --rep maven:/path/to/special/settings.xml com.example.foo

In Ceylon IDE, this option may be specified via `Lookup repositories on build path`
in the `Module Repositories` tab of the project-level compiler settings.

[`settings.xml`]: https://maven.apache.org/settings.html

### Gotcha!

You can't just pass the URL of a Maven repository using `--rep`. You _must_
specify it in `settings.xml`.

## Resolving Maven conflicts

Very often, when working with Maven repositories, we encounter one or all of
the following:

- versioning conflicts,
- undeclared dependencies, and/or
- the need to export promote transitive dependencies.

These problems arise from the fact that Maven metadata is often only tested
with a flat classpath, and breaks when executing on isolated classloaders in
Ceylon.

In such scenarios, there are two main ways to proceed:

- run your Ceylon program on a flat classpath, using the `--flat-classpath`
  flag of the command line tools, or
- use a [module overrides](../overrides) file to resolve the problems
  individually by adjusting the module dependencies.  

## Building with Maven

You can also [build your Ceylon modules using Maven](../../interoperability/maven), which is very useful when
you mix Java and Ceylon modules in your project.
