# aws-sdk-types-not-installed

Looks like clients depends on `@smithy/signature-v4`

```
npm ls @smithy/signature-v4            
vite-project@1.0.0 /Users/sboult/repro/aws-sdk-types-not-installed
└─┬ @aws-sdk/client-s3@3.523.0
  ├─┬ @aws-sdk/core@3.523.0
  │ └── @smithy/signature-v4@2.3.0
  ├─┬ @aws-sdk/middleware-sdk-s3@3.523.0
  │ └── @smithy/signature-v4@2.3.0 deduped
  ├─┬ @aws-sdk/middleware-signing@3.523.0
  │ └── @smithy/signature-v4@2.3.0 deduped
  └─┬ @aws-sdk/signature-v4-multi-region@3.523.0
    └── @smithy/signature-v4@2.3.0 deduped
```

Dep not in the package.json
```
cat node_modules/@aws-sdk/client-s3/package.json | grep @smithy/signature-v4
```

But is is installed in the core

```
cat node_modules/@aws-sdk/core/package.json | grep @smithy/signature-v4
```

However, there are types vended that depend on an explicit type that's not installed

```
grep -r "@smithy/signature-v4"  node_modules/@aws-sdk/client-s3/dist-types
```

Example from `dist-types`
```
node_modules/@aws-sdk/client-s3/dist-types/runtimeConfig.d.ts:    signerConstructor: (new (options: import("@smithy/signature-v4").SignatureV4Init & import("@smithy/signature-v4").SignatureV4CryptoInit) => import("@aws-sdk/types").RequestSigner) | typeof import("@aws-sdk/signature-v4-multi-region").SignatureV4MultiRegion;
node_modules/@aws-sdk/client-s3/dist-types/ts3.4/runtimeConfig.d.ts:        options: import("@smithy/signature-v4").SignatureV4Init &
node_modules/@aws-sdk/client-s3/dist-types/ts3.4/runtimeConfig.d.ts:          import("@smithy/signature-v4").SignatureV4CryptoInit
node_modules/@aws-sdk/client-s3/dist-types/ts3.4/runtimeConfig.native.d.ts:        options: import("@smithy/signature-v4").SignatureV4Init &
node_modules/@aws-sdk/client-s3/dist-types/ts3.4/runtimeConfig.native.d.ts:          import("@smithy/signature-v4").SignatureV4CryptoInit
node_modules/@aws-sdk/client-s3/dist-types/ts3.4/runtimeConfig.shared.d.ts:        options: import("@smithy/signature-v4").SignatureV4Init &
node_modules/@aws-sdk/client-s3/dist-types/ts3.4/runtimeConfig.shared.d.ts:          import("@smithy/signature-v4").SignatureV4CryptoInit
node_modules/@aws-sdk/client-s3/dist-types/ts3.4/runtimeConfig.browser.d.ts:        options: import("@smithy/signature-v4").SignatureV4Init &
node_modules/@aws-sdk/client-s3/dist-types/ts3.4/runtimeConfig.browser.d.ts:          import("@smithy/signature-v4").SignatureV4CryptoInit
node_modules/@aws-sdk/client-s3/dist-types/runtimeConfig.native.d.ts:    signerConstructor: (new (options: import("@smithy/signature-v4").SignatureV4Init & import("@smithy/signature-v4").SignatureV4CryptoInit) => import("@smithy/types").RequestSigner) | typeof import("@aws-sdk/signature-v4-multi-region").SignatureV4MultiRegion;
node_modules/@aws-sdk/client-s3/dist-types/runtimeConfig.shared.d.ts:    signerConstructor: (new (options: import("@smithy/signature-v4").SignatureV4Init & import("@smithy/signature-v4").SignatureV4CryptoInit) => import("@smithy/types").RequestSigner) | typeof SignatureV4MultiRegion;
node_modules/@aws-sdk/client-s3/dist-types/runtimeConfig.browser.d.ts:    signerConstructor: (new (options: import("@smithy/signature-v4").SignatureV4Init & import("@smithy/signature-v4").SignatureV4CryptoInit) => import("@smithy/types").RequestSigner) | typeof import("@aws-sdk/signature-v4-multi-region").SignatureV4MultiRegion;

```

