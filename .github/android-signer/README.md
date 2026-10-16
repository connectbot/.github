# Android signing policies

Public verification policies for ConnectBot's Hermit signing service. Files are
named by the immutable source repository ID, not a caller-supplied name. They
contain public certificates and allowed identities only. Private keys and their
filesystem locations stay in the appliance's private configuration.

Trusted workflows in `connectbot/actions` read this repository (immutable ID
1215286979) at protected `main`. They resolve a commit before fetching a policy
file and record the policy commit and digest in final provenance. Source builds
cannot replace this policy through workflow inputs or source checkout contents.

The attestor (`android-attest.yml`) and finalizer run from protected
`android-signer-v1` in
`connectbot/actions` (immutable ID 1369420724). Caller workflow paths identify
eligible CI entry points as a path allowlist. Exact executed workflow commits remain recorded in certificates,
receipts and provenance. Ordinary source/caller edits and reviewed builder branch
updates do not require changes here or on the VM.

Key identity, package, allowed source ref, signing algorithm/rotation policy,
Sigstore trust, or appliance revision changes require coordinated review and
provisioning. Export policies with the appliance's offline admin tool after key,
certificate and lineage validation; do not hand-edit fingerprints or policy hashes.
The test repository policy uses disposable keys. Its workflow cannot publish to
Google Play; production Play access is scoped to repository ID 123235 and alpha.

Application build instructions stay in each source repository. The shared
attestor verifies a stable artifact handoff without executing application code.
Changing Java, Gradle tasks, flavors, or output paths within that contract does
not require an update to these policies.

Only version 3 trusted-workflow policies are supported. Builder and finalizer
identities are mandatory; authorization mode switches, caller-file digests, and
workflow/helper content pins are not accepted. The native signer and shared
helpers must be updated together with these policy exports.
