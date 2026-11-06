Source: https://chromadotnet.org/security/

# Security

This page is about `ChromaDotNet.Client` and `ChromaDotNet.Client.DependencyInjection`, the packages the other ChromaDotNet libraries build on. The full policy is in [SECURITY.md](https://github.com/ChromaDotNet/ChromaDB.Client/blob/main/SECURITY.md).

## How the packages are published

  * Only the CI publishes the packages, from the version tags of [ChromaDotNet/ChromaDB.Client](https://github.com/ChromaDotNet/ChromaDB.Client), with [NuGet trusted publishing](https://learn.microsoft.com/nuget/nuget-org/trusted-publishing). No person and no repository keeps a NuGet API key.
  * The `ChromaDotNet.*` prefix is reserved on nuget.org for this organization.
  * The organization requires two-factor authentication.

## What every change goes through

  * `main` takes changes only through pull requests, and only when these checks pass: the tests on nine Chroma versions from 0.4.10 to 1.5.9, the trimming and NativeAOT build, the .NET Framework build, and CodeQL. The rule has no exceptions.
  * The workflows pin their actions to commit hashes.
  * Dependabot watches the dependencies and the actions, and [OpenSSF Scorecard](https://scorecard.dev/viewer/?uri=github.com/ChromaDotNet/ChromaDB.Client) rates the repository.

## What the packages depend on

  * `ChromaDotNet.Client`: System.Text.Json, System.Diagnostics.DiagnosticSource, System.Memory and Microsoft.Extensions.AI.Abstractions, all from Microsoft.
  * `ChromaDotNet.Client.DependencyInjection`: the client, Microsoft.Extensions.DependencyInjection.Abstractions and Microsoft.Extensions.Http.

## Verifying a package

The latest [GitHub release](https://github.com/ChromaDotNet/ChromaDB.Client/releases/latest) has the packages that the CI built from the version tag, with their Sigstore signatures and their build provenance. nuget.org serves the same packages with its repository signature added. So the two files have different hashes, and each check works on one of them.

On the package from the GitHub release, with Cosign 3 or later:
    
    
    cosign verify-blob ChromaDotNet.Client.2.20.0.nupkg --bundle ChromaDotNet.Client.2.20.0.nupkg.sigstore.json --certificate-identity-regexp '^https://github[.]com/ChromaDotNet/ChromaDB[.]Client/[.]github/workflows/release-assets[.]yml@' --certificate-oidc-issuer https://token.actions.githubusercontent.com
    

On the package from the GitHub release, with the GitHub CLI:
    
    
    gh attestation verify ChromaDotNet.Client.2.20.0.nupkg --bundle provenance-v2.20.0.intoto.jsonl --repo ChromaDotNet/ChromaDB.Client --signer-workflow ChromaDotNet/ChromaDB.Client/.github/workflows/ci.yml --source-ref refs/tags/v2.20.0
    

On the package from nuget.org:
    
    
    dotnet nuget verify --all ChromaDotNet.Client.2.20.0.nupkg
    

## Reporting a vulnerability

Do not report a vulnerability in a public issue. Report it privately on GitHub: [Report a vulnerability](https://github.com/ChromaDotNet/ChromaDB.Client/security/advisories/new). We acknowledge the report within 7 days, and confirm or rule it out within 14 days. Security fixes go into the latest release.
