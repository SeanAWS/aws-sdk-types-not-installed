# aws-sdk-types-not-installed

Looks like clients depends on `@smithy/signature-v4`

```
npm ls @smithy/signature-v4            
repro@1.0.0 /Volumes/workplace/aws-sdk-types-not-installed
└─┬ @aws-sdk/client-s3@3.850.0
  ├─┬ @aws-sdk/core@3.846.0
  │ └── @smithy/signature-v4@5.1.2
  ├─┬ @aws-sdk/middleware-sdk-s3@3.846.0
  │ └── @smithy/signature-v4@5.1.2 deduped
  └─┬ @aws-sdk/signature-v4-multi-region@3.846.0
    └── @smithy/signature-v4@5.1.2 deduped
```

Dep not in the package.json
```
cat node_modules/@aws-sdk/client-s3/package.json | grep @smithy/signature-v4
```

But it is installed in the core package

```
cat node_modules/@aws-sdk/core/package.json | grep @smithy/signature-v4
```

However, there are types vended that depend on an explicit type that's not installed

```
grep -r "@smithy/signature-v4"  node_modules/@aws-sdk/client-s3/dist-types
```

Example from `dist-types`
```
node_modules/@aws-sdk/client-s3/dist-types/runtimeConfig.d.ts:    signerConstructor: typeof import("@aws-sdk/signature-v4-multi-region").SignatureV4MultiRegion | (new (options: import("@smithy/signature-v4").SignatureV4Init & import("@smithy/signature-v4").SignatureV4CryptoInit) => import("@aws-sdk/types").RequestSigner);
node_modules/@aws-sdk/client-s3/dist-types/ts3.4/runtimeConfig.d.ts:        options: import("@smithy/signature-v4").SignatureV4Init &
node_modules/@aws-sdk/client-s3/dist-types/ts3.4/runtimeConfig.d.ts:          import("@smithy/signature-v4").SignatureV4CryptoInit
node_modules/@aws-sdk/client-s3/dist-types/ts3.4/runtimeConfig.native.d.ts:        options: import("@smithy/signature-v4").SignatureV4Init &
node_modules/@aws-sdk/client-s3/dist-types/ts3.4/runtimeConfig.native.d.ts:          import("@smithy/signature-v4").SignatureV4CryptoInit
node_modules/@aws-sdk/client-s3/dist-types/ts3.4/runtimeConfig.shared.d.ts:        options: import("@smithy/signature-v4").SignatureV4Init &
node_modules/@aws-sdk/client-s3/dist-types/ts3.4/runtimeConfig.shared.d.ts:          import("@smithy/signature-v4").SignatureV4CryptoInit
node_modules/@aws-sdk/client-s3/dist-types/ts3.4/runtimeConfig.browser.d.ts:        options: import("@smithy/signature-v4").SignatureV4Init &
node_modules/@aws-sdk/client-s3/dist-types/ts3.4/runtimeConfig.browser.d.ts:          import("@smithy/signature-v4").SignatureV4CryptoInit
node_modules/@aws-sdk/client-s3/dist-types/runtimeConfig.native.d.ts:    signerConstructor: typeof import("@aws-sdk/signature-v4-multi-region").SignatureV4MultiRegion | (new (options: import("@smithy/signature-v4").SignatureV4Init & import("@smithy/signature-v4").SignatureV4CryptoInit) => import("@smithy/types").RequestSigner);
node_modules/@aws-sdk/client-s3/dist-types/runtimeConfig.shared.d.ts:    signerConstructor: typeof SignatureV4MultiRegion | (new (options: import("@smithy/signature-v4").SignatureV4Init & import("@smithy/signature-v4").SignatureV4CryptoInit) => import("@smithy/types").RequestSigner);
node_modules/@aws-sdk/client-s3/dist-types/runtimeConfig.browser.d.ts:    signerConstructor: typeof import("@aws-sdk/signature-v4-multi-region").SignatureV4MultiRegion | (new (options: import("@smithy/signature-v4").SignatureV4Init & import("@smithy/signature-v4").SignatureV4CryptoInit) => import("@smithy/types").RequestSigner);
```

