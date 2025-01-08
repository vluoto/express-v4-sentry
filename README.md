# express-v4-sentry

- Node.js v22
- ESM
- Express v4
- Sentry JavaScript SDK v8

Run the app:

```shell
nvm use
npm install
node --import ./instrument.js .
```

```
[Sentry] express is not instrumented. This is likely because you required/imported express before calling `Sentry.init()`.
👂
```

Now, if you comment out lines related to `@sentry/profiling-node`:

```diff
diff --git a/instrument.js b/instrument.js
index 1c666d1..f9abdba 100644
--- a/instrument.js
+++ b/instrument.js
@@ -1,13 +1,13 @@
 import * as Sentry from "@sentry/node";
-import { nodeProfilingIntegration } from '@sentry/profiling-node';
+// import { nodeProfilingIntegration } from '@sentry/profiling-node';

 // Ensure to call this before importing any other modules!
 Sentry.init({
   dsn: "https://public@sentry.example.com/1",
-  integrations: [
-    // Add our Profiling integration
-    nodeProfilingIntegration(),
-  ],
+  // integrations: [
+  //   // Add our Profiling integration
+  //   nodeProfilingIntegration(),
+  // ],

   // Add Tracing by setting tracesSampleRate
   // We recommend adjusting this value in production
@@ -15,5 +15,5 @@ Sentry.init({

   // Set sampling rate for profiling
   // This is relative to tracesSampleRate
-  profilesSampleRate: 1.0,
+  // profilesSampleRate: 1.0,
 });

```

and try running the app again, you'll get no warning:

```shell
node --import ./instrument.js .
```

```
👂
```
