<h1>Could not load class : java.lang.NoClassDefFoundError</h1>

<!-- wp:paragraph -->
<p>When using the Local SDK, you will often times run into errors related to the ClassLaoder. A log will typically look like this. This article outlines a few causes and their solutions to this process.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>java.lang.IllegalArgumentException: Could not load class dev.getelements.robloxkit.service.StandardRobloxAuthService : java.lang.NoClassDefFoundError: dev/getelements/robloxkit/RobloxAuthService
	at io.github.classgraph.ScanResult.loadClass(ScanResult.java:1459)
	at io.github.classgraph.ScanResultObject.loadClass(ScanResultObject.java:228)
	at io.github.classgraph.ScanResultObject.loadClass(ScanResultObject.java:252)
	at io.github.classgraph.FieldInfo.loadClassAndGetField(FieldInfo.java:260)
	at java.base/</code></pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-code-is-missing-the-elementpublic-annotation">Code is missing the @ElementPublic annotation.</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Any class, interface, or package that is exposed as a service (via the <code>@ElementServiceExport</code> annotation) must also be made public. There's a few ways to do this.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Ensure All Related Types Are Public"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="c0945d"><div class="gb-notice-title" style="color:#fff"><p>Ensure All Related Types Are Public</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>The requirement to be public requires to the interface type itself as well as any types referenced within the interface.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-ensure-that-the-package-is-annotated-with-elementpublic">Ensure that the package is annotated with ElementPublic</h3>
<!-- /wp:heading -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-3fdb3f9a","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-3fdb3f9a { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-3fdb3f9a .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-3fdb3f9a .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-3fdb3f9a .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-3fdb3f9a .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-3fdb3f9a .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-3fdb3f9a .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"@ElementPublic\npackage com.example.mystudio.mygame;\n\nimport dev.getelements.elements.sdk.annotation.ElementPublic;","language":"java","fileName":"package-info.java"} /-->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-ensure-that-the-type-is-annotated-with-elementpublic">Ensure that the type is annotated with ElementPublic</h3>
<!-- /wp:heading -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-33c0b760","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-33c0b760 { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-33c0b760 .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-33c0b760 .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-33c0b760 .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-33c0b760 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-33c0b760 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-33c0b760 .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"package com.example.mystudio.mygame;\n\nimport dev.getelements.elements.sdk.annotation.ElementPublic;\n\n@ElementPublic\npublic interface MyInterface {\n\n void foo();\n\n}\n"} /-->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-a-required-dependency-is-missing">A Required Dependency is Missing</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Each Element must supply its own dependencies. This means that the Element must have included that dependency on the Classpath. Our example projects automate this process using the <a href="https://maven.apache.org/plugins/maven-dependency-plugin/copy-dependencies-mojo.html">Maven Copy Dependencies</a> Plugin and the local SDK expects those dependencies to be provided in <code>target/element-libs</code> relative to your project directory. The local SDK Maven runner will attempt to run Maven just before your code runs, but this may not always work correctly.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-check-your-pom-xml">Check your <code>pom.xml</code></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The pom should have a step to copy those dependencies in. The snippet in our example code ensures that only third-party code is included in that directory.</p>
<!-- /wp:paragraph -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-3d998542","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-3d998542 { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-3d998542 .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-3d998542 .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-3d998542 .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-3d998542 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-3d998542 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-3d998542 .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"            \u003cplugin\u003e\n                \u003cgroupId\u003eorg.apache.maven.plugins\u003c/groupId\u003e\n                \u003cartifactId\u003emaven-dependency-plugin\u003c/artifactId\u003e\n                \u003cversion\u003e3.6.0\u003c/version\u003e\n                \u003cexecutions\u003e\n                    \u003cexecution\u003e\n                        \u003cid\u003ecopy-element-deps\u003c/id\u003e\n                        \u003cphase\u003egenerate-resources\u003c/phase\u003e\n                        \u003cgoals\u003e\n                            \u003cgoal\u003ecopy-dependencies\u003c/goal\u003e\n                        \u003c/goals\u003e\n                        \u003cconfiguration\u003e\n                            \u003cprependGroupId\u003etrue\u003c/prependGroupId\u003e\n                            \u003cexcludeScope\u003eprovided\u003c/excludeScope\u003e\n                            \u003cexcludeGroupIds\u003ech.qos.logback\u003c/excludeGroupIds\u003e\n                            \u003cexcludeArtifactIds\u003esdk-local,sdk-local-maven,sdk-logback\u003c/excludeArtifactIds\u003e\n                            \u003coutputDirectory\u003e${project.build.directory}/element-libs\u003c/outputDirectory\u003e\n                        \u003c/configuration\u003e\n                    \u003c/execution\u003e\n                \u003c/executions\u003e\n            \u003c/plugin\u003e\n","language":"xml","fileName":"pom.xml"} /-->

<!-- wp:paragraph -->
<p>Typically, everything you need should appear in your IDE:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22276,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2026/01/image.png" alt="" class="wp-image-22276"/></figure>
<!-- /wp:image -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-jars-need-installed">Jars need Installed</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Because we use Maven to organize the local SDK, it will always copy from the jars installed in your <a href="https://maven.apache.org/repositories/local.html">Local Maven Repository</a>. In short a Maven Repository is a directory containing compiled Java code in an organized directory structure complete with version tagging. Just before your code runs, the LocalSDK copies pre-built code from that directory and copies it to your project. If you are running a project for the first time, you may have to manually build and install the code.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>To do this quickly, execute the following command in the root of your project:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>mvn -DskipTests clean install</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This tells Maven to clean, build, and install all jars in your project to your local Maven Repository. The <code>-DskipTests</code> ensures that integration and unit tests do not run, saving time and allowing you to debug code that may not be currently passing tests.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-special-cases-with-oas3-and-mongodb-drivers">Special Cases with OAS3 and MongoDB Drivers</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Server does everything it can to avoid forcing its opinions on the Elements. In doing so there are some oddities and nuance required with certain situations. Specifically when dealing with the following libraries:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>MongoDB Client Code</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>OpenAPI Annotations</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>To make some features work, we allow your code to inherit those opinions when necessary. We cover these cases here. In general, when using a type from the server, this must be done using the <code>@ElementTypeRequest</code> (3.7 and up) annotation. Additionally, you must ensure that the classpath entry is set to <code>provided</code>. This way the class does not appear in multiple places.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Future Proofing"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="69101b"><div class="gb-notice-title" style="color:#fff"><p>Future Proofing</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>This is not an exhaustive list. We do our best to keep this up to date. If you encounter these issues, please put them in the feedback section of this page, email us, or reach out in Discord so we can update.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-opinion-using-the-server-s-mongoclient-mongodatabase-or-openapi">Opinion: Using the Server's MongoClient, MongoDatabase, or OpenAPI</h3>
<!-- /wp:heading -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-34282627","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-34282627 { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-34282627 .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-34282627 .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-34282627 .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-34282627 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-34282627 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-34282627 .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"\u003cdependency\u003e\n    \u003cgroupId\u003eorg.mongodb\u003c/groupId\u003e\n    \u003cartifactId\u003emongodb-driver-core\u003c/artifactId\u003e\n    \u003cscope\u003eprovided\u003c/provided\u003e\n\u003c/dependency\u003e\n\u003cdependency\u003e\n    \u003cgroupId\u003eorg.mongodb\u003c/groupId\u003e\n    \u003cartifactId\u003emongodb-driver-sync\u003c/artifactId\u003e\n    \u003cscope\u003eprovided\u003c/provided\u003e\n\u003c/dependency\u003e\n","language":"xml","fileName":"pom.xml"} /-->

<!-- wp:paragraph -->
<p>When you want to use the actual share connection with the database, you are inheriting the <code>MongoClient</code> from the Elements server core. To do so you must ensure that your pom does not include the jar on the classpath of the Element. You must ensure that it is included as a provided dependency.</p>
<!-- /wp:paragraph -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-d962a492","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-d962a492 { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-d962a492 .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-d962a492 .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-d962a492 .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-d962a492 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-d962a492 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-d962a492 .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"@ElementPublic\n@ElementTypeRequest(\u0022com.mongodb.client.MongoClient\u0022)\npackage com.mystudio.mygame;\n\nimport dev.getelements.elements.sdk.annotation.ElementTypeRequest;\n\nimport dev.getelements.elements.sdk.annotation.ElementPublic;\n","language":"java","fileName":"package-info.java"} /-->

<!-- wp:paragraph -->
<p>These combined indicate htat because the scope is <code>provided</code> the jars do not get packed in the Element and the <code>ElementTypeRequest</code> indicates that they will be made available to your Element. <strong>In most cases, this is what you want</strong>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Opinion: Providing your Own Types</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If you wish to use your own types, you may do so by including the drivers in the compiled scope. This ensures you are able to include the code in the Element. If you do not set the scope, then these will not appears in the resulting bundle.</p>
<!-- /wp:paragraph -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-0bppxw7","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-0bppxw7 { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-0bppxw7 .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-0bppxw7 .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-0bppxw7 .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-0bppxw7 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-0bppxw7 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-0bppxw7 .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"\u003cdependency\u003e\n    \u003cgroupId\u003eorg.mongodb\u003c/groupId\u003e\n    \u003cartifactId\u003emongodb-driver-core\u003c/artifactId\u003e\n    \u003cscope\u003ecompile\u003c/provided\u003e\n\u003c/dependency\u003e\n\u003cdependency\u003e\n    \u003cgroupId\u003eorg.mongodb\u003c/groupId\u003e\n    \u003cartifactId\u003emongodb-driver-sync\u003c/artifactId\u003e\n    \u003cscope\u003ecompile\u003c/provided\u003e\n\u003c/dependency\u003e\n","language":"xml","fileName":"pom.xml"} /-->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Sharing Types"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="e782c0"><div class="gb-notice-title" style="color:#fff"><p>Sharing Types</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Do not add these types to your Element if you wish to return or hand the type with the server. You will likely get an instance of <code>ClassCastExcpption</code> for the same types. eg <code>Cannoot cast com.mongodb.client.MongoDatabase to com.mongodb.client.MongoDatabase</code>.  </p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p><strong>In most cases you do not want this, unless you have a particularly advanced use case.</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
