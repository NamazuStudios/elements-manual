<h1>Unable to deploy application : dev.getelements.elements.sdk.exception.SdkElementNotFoundException</h1>

<!-- wp:paragraph -->
<p>This happens when the LocalSDK is not able to read the element's code at all. This means that no code at all can be found and the loder is giving up entirely. Typically you see this error in the logs:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>dev.getelements.elements.sdk.exception.SdkElementNotFoundException: null
at java.base/java.util.Optional.orElseThrow(Optional.java:403)
at dev.getelements.elements.sdk.local.internal.LocalApplicationElementService.doLoadElement(LocalApplicationElementService.java:102)
at java.base/java.util.stream.ReferencePipeline$3$1.accept(ReferencePipeline.java:197)
at java.base/java.util.stream.ReferencePipeline$2$1.accept(ReferencePipeline.java:179)
at java.base/java.util.AbstractList$RandomAccessSpliterator.forEachRemaining(AbstractList.java:722)
at java.base/java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:509)
at java.base/java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:499)
at java.base/java.util.stream.AbstractPipeline.evaluate(AbstractPipeline.java:575)
at java.base/java.util.stream.AbstractPipeline.evaluateToArrayNode(AbstractPipeline.java:260)
at java.base/java.util.stream.ReferencePipeline.toArray(ReferencePipeline.java:616)
at java.base/java.util.stream.ReferencePipeline.toArray(ReferencePipeline.java:622)
at java.base/java.util.stream.ReferencePipeline.toList(ReferencePipeline.java:627)
at dev.getelements.elements.sdk.local.internal.LocalApplicationElementService.lambda$getOrLoadApplication$1(LocalApplicationElementService.java:65)</code></pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-missing-elementdefinition-annotation">Missing <code>@ElementDefinition</code> Annotation</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The annotation is missing. Add to your <code>package-info.java</code></p>
<!-- /wp:paragraph -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-0d636cf5","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-0d636cf5 { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-0d636cf5 .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-0d636cf5 .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-0d636cf5 .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-0d636cf5 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-0d636cf5 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-0d636cf5 .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"@ElementDefinition\npackage com.mystudio.mygame;\n\nimport dev.getelements.elements.sdk.annotation.ElementDefinition;\n","fileName":"package-info.java"} /-->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-check-that-the-correct-element-is-requested-local-sdk-only">Check that the correct Element is Requested (Local SDK Only)</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The example Element using the LocalSDK uses a block of code to load. Make sure that you have specified the correct Element to load, or else you will see this error:</p>
<!-- /wp:paragraph -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-057488b7","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-057488b7 { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-057488b7 .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-057488b7 .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-057488b7 .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-057488b7 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-057488b7 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-057488b7 .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"        // Create the local instance of the Elements server\n        final var local = ElementsLocalBuilder.getDefault()\n                .withElementNamed(\n                        \u0022example\u0022,\n                        \u0022com.mystudio.mygame\u0022,\n                        PropertiesAttributes.wrap(elementProperties))\n                .build();\n\n","language":"java","fileName":"Main.java"} /-->

<!-- wp:paragraph -->
<p>In this case, change the <code>com.mystudio.mygame</code> to the correct Package or Element where the <code>@ElementDefinition</code> exists.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
