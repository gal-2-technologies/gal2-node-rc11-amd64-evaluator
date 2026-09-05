# GAL-2 Node RC11 Linux AMD64 Security and Deployment Handoff

Document version: 1.0  
Document date: 2026-09-05  
Release: `v1.0.0-rc11-amd64-evaluator`  
Audience: independent technical and security evaluators

## 1. Purpose and scope

This document provides the release identity, provenance boundary, security
status, network behavior, host modifications, deployment sequence, fault
injection behavior, failure criteria, and cleanup procedure for the GAL-2 Node
RC11 Linux AMD64 evaluator.

This is a controlled technical evaluation candidate. It is not a declaration
of general production readiness, independent certification, native AMD64
performance qualification, or a completed third-party security review.

The intended product boundary is:

```text
reference inputs -> Protected Core -> GAL-2 Time -> API -> Node -> Time Contract -> enrolled application
```

GAL-2 does not replace or discipline the host clock, NTP, PTP, GNSS, chrony,
or systemd-timesyncd. An enrolled application consumes GAL-2 Time through the
GAL-2 Provider. The Time Contract field `safe_to_consume` is the authoritative
application-consumption decision.

## 2. Direct answers to evaluator due-diligence questions

| Question | Answer for this release |
| --- | --- |
| Is there an official download? | Yes. The frozen evaluator is distributed through the GitHub release identified below. |
| Is the download cryptographically identified? | Yes. The outer ZIP, signed package, detached signature, public key, evidence archive, runtime archive, OCI manifest, OCI config, and tested payload are identified by SHA-256. |
| Is the package signed? | Yes. The signed package has a detached OpenPGP signature. The expected primary fingerprint is published below. |
| Does a good signature prove trust in the supplied key? | No. It proves cryptographic validity against that key. The evaluator must verify the primary fingerprint through an independent trusted GAL-2 channel. |
| Is the distributed runtime built solely from source in the public evaluator repository? | No. The public repository does not contain the GAL-2 Protected Core, the complete internal runtime source, or a Dockerfile capable of rebuilding the frozen OCI image. |
| What does `Reproducible build: PASS` mean? | It applies to deterministic evaluator-package assembly over frozen inputs. It does not mean the OCI runtime can be independently rebuilt from the public repository. |
| Is an SBOM included? | No formal SPDX, CycloneDX, or equivalent SBOM is included in RC11. |
| Is a formal vulnerability scan report included? | No. RC11 does not include a formal CVE or container-vulnerability scan report. Absence of a report must not be interpreted as absence of vulnerabilities. |
| Was there an independent security audit? | No. The bundled audit and external-evaluation evidence are founder-operated. This handoff requests independent evaluation. |
| Does the package contain an API key? | No. A paid or temporary evaluator credential is supplied separately through a private channel. |
| What remote application destination is used? | Static inspection found one application-level remote destination: `https://api-v2.gal-2.com/time`. |
| Is telemetry sent to another destination? | No separate analytics, crash-reporting, or telemetry destination was found in the frozen GAL-2 application source. A local request counter is maintained, and the GAL-2 API necessarily observes authenticated API requests. |
| Does the Node alter the host timing stack? | No host-clock mutation primitive was found in the packaged deployment or evaluation scripts. The harness records timing-service state before and after the run and requires it to remain unchanged. |
| Is Podman supported? | No. Podman and `podman-docker` are not supported by this RC11 package. Genuine Docker Engine and `docker.service` are required. |

## 3. Official release identity

Official release:

<https://github.com/gal-2-technologies/gal2-node-rc11-amd64-evaluator/releases/tag/v1.0.0-rc11-amd64-evaluator>

Required public downloads:

```text
GAL2_NODE_LINUX_AMD64_RC11_EXTERNAL_AUDIT_FINAL_20260902.zip
GAL2_NODE_LINUX_AMD64_RC11_EXTERNAL_AUDIT_FINAL_20260902.zip.sha256
```

GitHub-generated `Source code (zip)` and `Source code (tar.gz)` downloads are
not the evaluator package.

### 3.1 Release artifacts

| Identity | Value |
| --- | --- |
| Release tag | `v1.0.0-rc11-amd64-evaluator` |
| Package source revision | `a22098a01462917ac4923ea9ffdb0a0f56e5c328` |
| Final audit ZIP | `GAL2_NODE_LINUX_AMD64_RC11_EXTERNAL_AUDIT_FINAL_20260902.zip` |
| Final audit ZIP SHA-256 | `0db64868d6bfe1a7230a0e675fd24aec096e45a916e1813dcaac4d2d534efefb` |
| Signed package | `GAL2_NODE_LINUX_AMD64_1.0.0-rc11-evaluator.tar.gz` |
| Signed package size | `45323256` bytes |
| Signed package SHA-256 | `d452a276af1bf4bd2abb11dfa01f14f22f7c72f36eda9a3019c4269ef36ef553` |
| Detached signature SHA-256 | `5c56f92df04f37d73b13625f06aafe2d1faf2d5f199c3007982f73aef2a48773` |
| Release public key SHA-256 | `4342e36099b65889fc18fe06ef01063595ee15bd0d2d3df6cd3d81d4b28f1bfc` |
| External evidence SHA-256 | `7777d141ef05245ff2cd76cba8eece9735b03474c134ecfb2f08bc5ee66d305f` |
| Expected signing fingerprint | `802C 8978 FF85 7550 60B6 D6BC 8AB8 59E4 D705 822F` |

The final audit ZIP contains exactly seven files:

```text
GAL2_NODE_LINUX_AMD64_1.0.0-rc11-evaluator.tar.gz
GAL2_NODE_LINUX_AMD64_1.0.0-rc11-evaluator.tar.gz.sha256
GAL2_NODE_LINUX_AMD64_1.0.0-rc11-evaluator.tar.gz.asc
GAL2_RELEASE_SIGNING_PUBLIC_KEY_RC11.asc
AUDIT_IDENTITY.txt
GAL2_NODE_LINUX_AMD64_RC11_EXTERNAL_EVIDENCE_A22098A_20260902.tar.gz
GAL2_NODE_LINUX_AMD64_RC11_EXTERNAL_EVIDENCE_A22098A_20260902.tar.gz.sha256
```

### 3.2 Frozen runtime identity

| Identity | Value |
| --- | --- |
| Runtime source revision | `c8515d4feceda6eaa6fb3389f41a0b729493fc24` |
| Runtime archive SHA-256 | `30361a33110d29b52c2d90ab5ddc0f0a479a60356a079fef3acdbbf87d901ad2` |
| OCI manifest digest | `sha256:497ab1646033ecbb28504489c874f6eda80b12507a7bd3ca4f2cb498fb471f9d` |
| OCI config digest | `sha256:abc2c606cba0e2b755398c51fd6919e5d8f2c880ebda11def76f6c8be567ad9f` |
| OCI layers | `11` |
| Platform | `linux/amd64` |
| Original tested tag | `gal2-node:rc11-amd64-evaluator-c8515d4` |
| Time Contract | `1.2.0-contract-rc.4` |
| SHM layout | `1.0` |
| Frozen tested-payload manifest SHA-256 | `2ff1e8a8dfc3a80bce135727d921b760824c60d939c8321442d5b8788f444661` |
| Tested-payload checksum-set SHA-256 | `f5ec86cfdc9334226f11c5738593c888974561695a26d6018c267a57be5bcd76` |

The identity policy declares:

```text
consumer_payload_byte_for_byte_frozen=true
runtime_archive_byte_for_byte_frozen=true
runtime_rebuild_allowed_after_freeze=false
validation_evidence_is_runtime_payload=false
```

### 3.3 Critical deployment component identities

| Component | SHA-256 |
| --- | --- |
| Package builder | `f48a84f6421630ddd596aa5ffac1883f98e81b4943280de5117055320309ae18` |
| Installer | `dc0e460f5172f127823081e2a9f37b78ef30c1cd24cecc7b80de7f17ab96b34e` |
| Uninstaller | `174f61680d79c9bc8abab549141939f9a378777b4a065551dcb2e613fbe14465` |
| `gal2-node` control command | `9e12d879e77a39a464becbf64e72dc87650876ec9cab55af385f716279dd5aab` |
| Container guard | `edad5fcdfa24eb53ea558a8068c8afce9faad000609500c1a7c7d4511d8cc4c0` |
| Fault-injection firewall helper | `83997a04b693cd71539870b0242825d3c2db684e3c8b4c93ebd9b0b5efc3db64` |
| External evaluation harness | `87824d26bf41d746d2c9b57fe62a3a216fdbe4bf2a07ec095262ca4dc683ca7d` |
| systemd unit template | `bd87d969fdd9433e01424289edc4f38895f9a14aa9e41039bbb15b08e2049ebf` |

## 4. Independent download and signature verification

Run these commands from the download directory:

```bash
sha256sum --check \
  GAL2_NODE_LINUX_AMD64_RC11_EXTERNAL_AUDIT_FINAL_20260902.zip.sha256

mkdir gal2-rc11-amd64-evaluation
unzip GAL2_NODE_LINUX_AMD64_RC11_EXTERNAL_AUDIT_FINAL_20260902.zip \
  -d gal2-rc11-amd64-evaluation
cd gal2-rc11-amd64-evaluation

sha256sum --check \
  GAL2_NODE_LINUX_AMD64_1.0.0-rc11-evaluator.tar.gz.sha256
sha256sum --check \
  GAL2_NODE_LINUX_AMD64_RC11_EXTERNAL_EVIDENCE_A22098A_20260902.tar.gz.sha256
```

Verify the detached signature with an isolated temporary GPG home:

```bash
(
  set -e
  verify_home="$(mktemp -d)"
  trap 'rm -rf -- "$verify_home"' EXIT
  chmod 700 "$verify_home"

  gpg --batch \
    --homedir "$verify_home" \
    --import GAL2_RELEASE_SIGNING_PUBLIC_KEY_RC11.asc

  gpg --batch \
    --homedir "$verify_home" \
    --fingerprint

  gpg --batch \
    --homedir "$verify_home" \
    --verify \
    GAL2_NODE_LINUX_AMD64_1.0.0-rc11-evaluator.tar.gz.asc \
    GAL2_NODE_LINUX_AMD64_1.0.0-rc11-evaluator.tar.gz
)
```

Required results:

- GPG reports a good signature.
- The primary fingerprint exactly matches
  `802C 8978 FF85 7550 60B6 D6BC 8AB8 59E4 D705 822F`.
- A warning that the key is not certified in the evaluator's temporary
  keyring is expected. Trust in the signing identity still requires an
  independent fingerprint comparison.

### 4.1 Independent fingerprint channel

For this evaluation, compare the fingerprint published in the GitHub handoff
with the same fingerprint stated directly in a message from a GAL-2 corporate
email address under `gal-2.com`. Confirm the sender through the LinkedIn
account used to arrange the evaluation. The GitHub publication and the
separately confirmed corporate message are distinct transport channels.

The copy of the fingerprint inside the downloaded ZIP is useful corroborating
metadata, but it is not an independent channel because it travels with the
artifact being verified.

Extract and verify the tested payload:

```bash
tar -xzf GAL2_NODE_LINUX_AMD64_1.0.0-rc11-evaluator.tar.gz
cd GAL2_NODE_LINUX_AMD64_1.0.0-rc11-evaluator
sha256sum --check SHA256SUMS.tested-payload.txt
```

Every listed file must report `OK`.

## 5. Build and source provenance boundary

The release separates package assembly identity from runtime build identity.

### 5.1 What is demonstrated

- The outer evaluator package has a declared package source revision.
- `PACKAGE_MANIFEST.json` declares a 21-file source allowlist plus generated
  package metadata.
- Packaged files have declared paths, sizes, modes, and SHA-256 values.
- `scripts/build_package.py` assembles the evaluator package over a previously
  frozen runtime archive and verifies the expected runtime archive SHA-256.
- The runtime OCI archive, manifest, config, layers, labels, platform, and
  tested consumer payload are cryptographically fixed.
- The installer refuses an unexpected archive, OCI graph, Docker object,
  platform, label set, file identity, or file mode.
- `AUDIT_IDENTITY.txt` records `Reproducible build: PASS` for this package
  assembly process.

### 5.2 What is not demonstrated publicly

- The public evaluator repository is not the complete source repository for
  the runtime.
- The GAL-2 Protected Core and backend are not included.
- The runtime Dockerfile and complete internal build recipe are not included.
- An evaluator cannot rebuild the OCI image solely from the public repository.
- The package and runtime source revisions are GAL-2 internal provenance
  declarations. They are not proof of equivalence to a complete public source
  tree.
- No third-party build attestation or independent reproducible-build result is
  included.

The accurate claim is therefore: the evaluator package is reproducibly
assembled over frozen, cryptographically identified inputs. It is not a fully
publicly reproducible build of the protected runtime.

## 6. Security assurance status

### 6.1 Included checks and evidence

- SHA-256 identity for the distribution and nested artifacts
- Detached OpenPGP signature and fixed expected primary fingerprint
- Package source allowlist and exact file modes
- Tested-payload checksum verification
- OCI graph and blob verification
- OCI platform and label verification
- Docker object identity verification after load
- Installed file ownership, mode, and hash validation
- Source secret scan recorded as `PASS`
- Evidence secret scan recorded as `PASS`
- Evidence-structure validation recorded as `PASS`
- Controlled functional and fault-injection evidence
- Refusal-oriented uninstall and purge ownership checks

### 6.2 Not included in RC11

- Formal SPDX or CycloneDX SBOM
- Formal container or dependency vulnerability scan report
- Independent penetration test
- Independent source-code security audit
- Independent supply-chain audit
- Publicly reproducible build of the protected OCI runtime
- Production certification

These absences are disclosed limitations. They must not be converted into
claims that no vulnerabilities exist.

### 6.3 Visible Python distribution inventory

Static inspection of the exact frozen runtime found the following installed
Python distributions. This is supplemental inventory, not an exhaustive SBOM
of the operating-system packages, runtime, or build chain.

```text
blinker==1.9.0
certifi==2026.7.22
charset-normalizer==3.4.9
click==8.4.2
Flask==3.1.3
idna==3.18
itsdangerous==2.2.0
Jinja2==3.1.6
MarkupSafe==3.0.3
pip==26.2.1
requests==2.34.2
urllib3==2.7.0
Werkzeug==3.1.8
```

## 7. Known static-analysis artifact

The frozen OCI runtime contains this generated bytecode file:

```text
/opt/gal2/gate2/src/__pycache__/gal2_ixoye_observer.cpython-313.pyc
```

The corresponding source is included in the same frozen OCI runtime:

```text
/opt/gal2/gate2/src/gal2_ixoye_observer.py
```

For a scanner that recognizes a URL across the intervening marshalled bytes,
this exact `.pyc` deterministically produces the following concatenated
finding. Expect it from permissive byte-oriented URL extraction:

```text
http://127.0.0.1:9095/contractz(/usr/local/gal2/evidence/ixoye_live_rc57z
```

The source contains two separate values:

```python
DEFAULT_CONTRACT_URL = "http://127.0.0.1:9095/contract"
DEFAULT_OUTPUT_DIR = "/usr/local/gal2/evidence/ixoye_live_rc57"
```

The actual observer network operation uses `urlopen` against the configured
contract URL, whose default is the loopback endpoint. It does not use the
concatenated string reported by byte-oriented scanners.

The file is timestamp-based bytecode. Its embedded source timestamp and source
size match the included `gal2_ixoye_observer.py`. The image also sets
`PYTHONDONTWRITEBYTECODE=1`; that setting prevents new bytecode writes but does
not prohibit Python from loading valid existing bytecode.

The normal OCI command runs `/opt/gal2/gate2/src/gal2d.py`. Static inspection
found no import or invocation of `gal2_ixoye_observer` from `gal2d.py`, so the
normal Node entrypoint is not shown to load this `.pyc`. The bytecode remains
potentially loadable if that observer module is explicitly imported or
invoked, and it is therefore disclosed as part of the image's static-analysis
and attack surface.

The `.pyc` is not individually listed as an outer package source file, but it
is not outside the release identity. It is transitively frozen by its OCI
layer digest, the OCI manifest digest, and the runtime archive SHA-256.

Generated-bytecode removal and an explicit build-time rejection invariant are
tracked as RC12 build-hygiene and static-analysis improvements.

## 8. Runtime network behavior and telemetry

### 8.1 Host-facing interface

The process binds to port 9095 inside the container. Docker publishes it only
on host loopback:

```text
127.0.0.1:9095:9095
```

Principal local interfaces include:

```text
http://127.0.0.1:9095/health
http://127.0.0.1:9095/contract
/run/gal2/contract-v1.shm
```

The service is not published on a non-loopback host interface.

### 8.2 Remote destination

Static inspection of the frozen GAL-2 application source found one actual
application-level remote request:

```text
GET https://api-v2.gal-2.com/time
x-api-key: <configured credential>
```

Default request behavior includes:

| Setting | Default |
| --- | --- |
| Active refresh | 30 seconds |
| Idle refresh | 30 seconds |
| Holdover retry | 10 seconds |
| HTTP timeout | 10 seconds |

Normal operation therefore requires DNS resolution and outbound HTTPS on TCP
port 443 to `api-v2.gal-2.com`.

### 8.3 Non-destination URL strings

The runtime also contains these schema identifiers:

```text
https://gal-2.com/schemas/gal2-time-contract-v1.schema.json
https://json-schema.org/draft/2020-12/schema
```

They identify JSON schemas. No application network call using either value was
found in the inspected runtime source.

The IXOYE observer's default URL is local:

```text
http://127.0.0.1:9095/contract
```

### 8.4 Telemetry boundary

- No separate remote analytics, advertising, crash-reporting, or telemetry
  destination was found in the inspected application source.
- Structured application logs are written to the container log stream and are
  available locally through `journalctl` and `gal2-node logs`.
- A local request count is persisted in `/var/lib/gal2/gal2_usage.json`.
- The configured GAL-2 API necessarily receives authenticated synchronization
  requests and can enforce the associated API entitlement and quota.
- The service loads the frozen image locally and uses `--pull never`; normal
  Node startup does not pull an image from a registry.

## 9. Host-side modifications

The installer requires root privileges and performs the following changes:

| Host object | Purpose and expected state |
| --- | --- |
| `/etc/gal2/` | Dedicated configuration directory, root-owned, mode `0700` |
| `/etc/gal2/node.env` | API and Node configuration, root-owned, mode `0600`; created with an empty API key if absent |
| `/var/lib/gal2/` | Persistent cache and local request state, root-owned, mode `0700` |
| `/run/gal2/` | Runtime directory and SHM publication, mode `0755` |
| `/run/gal2/contract-v1.shm` | Application-facing Time Contract SHM record, 8192 bytes, normally mode `0644` |
| `/usr/local/bin/gal2-node` | Control command, root-owned, mode `0755` |
| `/usr/local/lib/gal2-node/` | Installed SDK, demo, guard, uninstaller, manifests, and documentation, root-owned, mode `0755` |
| `/etc/systemd/system/gal2-node.service` | Rendered systemd unit, root-owned, mode `0644` |
| systemd enablement link | Created by `systemctl enable gal2-node.service` |
| Docker image | Exact frozen image tagged `gal2-node:rc11-amd64-evaluator-c8515d4` |
| Docker container | Named `gal2-node` while the service is running; launched with `--rm` |

The installer enables `gal2-node.service` but does not start it automatically.
The evaluator must configure the credential and explicitly start the service.

If the exact GAL-2 service is already active, the installer stops it before
replacing the unit. A clean disposable evaluation host is recommended to avoid
pre-existing state or name collisions.

### 9.1 SELinux behavior

The systemd unit uses Docker private relabeling on dedicated GAL-2 paths:

```text
/run/gal2:/run/gal2:Z
/var/lib/gal2:/var/lib/gal2:Z
```

On an SELinux-enforcing host, `:Z` can change the SELinux labels on those two
dedicated host directories so that the private container can access them. The
installer does not disable SELinux. Native RHEL 10.2 behavior, including
host-side Provider access to the SHM file after relabeling, is part of the
requested independent compatibility evaluation.

### 9.2 Actions the package does not perform

- It does not set the host time.
- It does not call a host-clock mutation primitive.
- It does not configure or restart chronyd, NTP, PTP, GNSS, or
  systemd-timesyncd.
- It does not use host networking.
- It does not mount the Docker socket inside the container.
- It does not run the container with `--privileged`.
- It drops all Linux capabilities.
- It enables `no-new-privileges`.
- It does not expose port 9095 beyond host loopback.
- It does not automatically start the Node before the API credential is
  configured.

## 10. Deployment architecture

The systemd unit requires `docker.service` and uses this security and runtime
model:

```text
systemd
  -> Docker Engine
     -> bridge-networked gal2-node container
        -> HTTPS synchronization with api-v2.gal-2.com
        -> local loopback HTTP contract
        -> bind-mounted SHM and persistent state
```

Important Docker flags:

```text
--rm
--pull never
--network bridge
--cap-drop ALL
--security-opt no-new-privileges:true
--publish 127.0.0.1:9095:9095
--volume /run/gal2:/run/gal2:Z
--volume /var/lib/gal2:/var/lib/gal2:Z
```

The prestart and stop guard validates the container name, image object,
expected tag, and GAL-2 ownership labels before taking action. Unexpected
objects cause refusal rather than deletion or replacement.

## 11. RHEL 10.2 x86_64 evaluation environment

### 11.1 Docker support boundary

Red Hat does not ship or support Docker Engine as the RHEL container engine.
RHEL uses Podman. Docker's own documentation currently lists maintained RHEL
10 as an installation target for Docker Engine.

References:

- [Docker Engine installation on RHEL](https://docs.docker.com/engine/install/rhel/)
- [Red Hat RHEL 10 container-engine boundary](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/building_running_and_managing_containers/introduction-to-containers)

RC11 requires genuine Docker Engine 28.3.3 or newer and an active
`docker.service`. Do not substitute Podman or `podman-docker`.

Recommended first target:

- Disposable RHEL 10.2 x86_64 VM
- VM hosted on native x86_64 hardware
- Snapshot taken before Docker or GAL-2 installation
- SELinux left in its normal enforcing state
- Docker Engine installed using Docker's documented RHEL procedure
- No production workloads on the host

A VM on native x86_64 hardware is sufficient for initial installation,
SELinux, systemd, Docker, SHM, contract, Provider, and fault-injection
compatibility testing. Bare-metal x86_64 is preferred for later timing,
latency, scheduling, and endurance characterization.

Docker installation itself can modify host networking, forwarding, firewall
rules, packages, and services. Those are Docker platform changes, not changes
performed by the GAL-2 installer. Use a disposable host and record the Docker
configuration before installing GAL-2.

### 11.2 Preflight capture

Run before GAL-2 installation:

```bash
uname -m
cat /etc/redhat-release
uname -r
getenforce

systemctl is-active docker
systemctl is-enabled docker
sudo docker version
sudo docker info
sudo docker network inspect bridge >/dev/null && echo DOCKER_BRIDGE=PASS

command -v python3
command -v curl
command -v sha256sum
command -v tar
command -v unzip
command -v gpg
```

For the complete fault-injection harness, also record:

```bash
command -v iptables
sudo iptables -w 5 -nL DOCKER-USER
systemctl is-active firewalld || true
sudo firewall-cmd --state || true
```

Expected architecture:

```text
x86_64
```

If `iptables -nL DOCKER-USER` fails, report the Docker firewall backend,
firewalld state, command output, and host configuration before proceeding to
fault injection. Do not disable firewalld, change the backend, or add an
unreviewed compatibility rule merely to force the evaluation to pass.

Docker supports iptables and nftables firewall backends. The supplied RC11
fault helper specifically requires the `iptables` command and Docker-managed
`DOCKER-USER` chain. A Docker native nftables configuration without that chain
is outside this helper's supported fault-injection path.

References:

- [Docker packet filtering and firewalls](https://docs.docker.com/engine/network/packet-filtering-firewalls/)
- [Docker with nftables](https://docs.docker.com/engine/network/firewall-nftables/)

## 12. Exact deployment and baseline sequence

### 12.1 Review the plan before host mutation

From the extracted package directory:

```bash
python3 evaluation/run_external_evaluation.py --plan-only
```

Required final line:

```text
GAL2_EXTERNAL_EVALUATION_PLAN=PASS
```

### 12.2 Install without starting

```bash
sudo ./scripts/install.sh
```

The installer validates:

- Tested payload checksums
- Frozen manifest
- Runtime archive identity
- OCI graph and all blob digests
- OCI labels and Linux AMD64 platform
- Loaded Docker object identity
- Installed ownership and modes

Required post-install state on a clean host:

```text
gal2-node.service enabled
gal2-node.service inactive
GAL2_API_KEY empty until separately configured
```

### 12.3 Configure the credential

```bash
sudoedit /etc/gal2/node.env
```

Set the separately supplied credential:

```text
GAL2_API_KEY=YOUR_GAL2_API_KEY
```

Never place the key in a repository, command transcript, issue, screenshot,
evidence archive, or evaluation report.

Verify metadata without printing the credential:

```bash
sudo stat -c 'OWNER=%U:%G MODE=%a PATH=%n' /etc/gal2/node.env
```

Required result:

```text
OWNER=root:root MODE=600 PATH=/etc/gal2/node.env
```

### 12.4 Start and establish baseline

```bash
sudo gal2-node start
sleep 40
sudo gal2-node doctor
gal2-node contract
```

A consumption-ready baseline must include:

```text
shm_publication=PASS
gal2_time_ready=PASS
DOCTOR=PASS
mode=LIVE
safe_to_consume=true
reason=fresh_api_sync
```

Run the supplied enrolled-application Provider demo:

```bash
PYTHONPATH=/usr/local/lib/gal2-node/sdk/python \
python3 /usr/local/lib/gal2-node/examples/python/gal2_demo.py
```

Required application-level properties:

```text
status=ok
safe_to_consume=true
raw_host_time_fallback=false
```

On SELinux-enforcing RHEL, record whether the host-side Provider can read:

```text
/run/gal2/contract-v1.shm
```

Also capture:

```bash
ls -lZ /run/gal2 /run/gal2/contract-v1.shm /var/lib/gal2
sudo docker inspect gal2-node --format \
  'status={{.State.Status}} image={{.Config.Image}} network={{.HostConfig.NetworkMode}} global_ipv6={{range .NetworkSettings.Networks}}{{.GlobalIPv6Address}}{{end}}'
```

## 13. Controlled external evaluation

Run from the extracted package directory with a new evidence path that does
not already exist:

```bash
sudo python3 evaluation/run_external_evaluation.py \
  --execute \
  --evidence-dir /tmp/gal2-rc11-external-evidence \
  --release-archive ../GAL2_NODE_LINUX_AMD64_1.0.0-rc11-evaluator.tar.gz \
  --expected-release-sha256 d452a276af1bf4bd2abb11dfa01f14f22f7c72f36eda9a3019c4269ef36ef553 \
  --release-signature ../GAL2_NODE_LINUX_AMD64_1.0.0-rc11-evaluator.tar.gz.asc \
  --release-public-key ../GAL2_RELEASE_SIGNING_PUBLIC_KEY_RC11.asc
```

### 13.1 Policy used by the accelerated harness

| Policy | Normal default | Accelerated evaluation |
| --- | ---: | ---: |
| Holdover soft limit | 21600 seconds, 6 hours | 60 seconds |
| Holdover hard limit | 259200 seconds, 72 hours | 90 seconds |
| Rejoin guard | 10 seconds | 10 seconds |
| LIVE validity | 45 seconds | 45 seconds |
| Provider unsafe propagation timeout | Not applicable | 10 seconds |

The accelerated limits make state transitions testable in minutes. They do
not claim equivalence to a real-duration 72-hour native AMD64 endurance run.
The harness must restore the normal policy afterward.

### 13.2 Fault-injection mechanism

The helper:

- Requires root, `iptables`, Docker bridge networking, and `DOCKER-USER`.
- Resolves the current evaluator container IPv4 address.
- Refuses non-bridge networking.
- Refuses a container with a global IPv6 address because IPv4-only isolation
  would be incomplete.
- Creates the dedicated chain `GAL2_EVALUATOR_API_BLOCK`.
- Inserts a source-address rule at the beginning of `DOCKER-USER`.
- Rejects all forwarded IPv4 egress from that evaluator container.
- Stores managed state under `/run/gal2-evaluator-firewall/` with restricted
  ownership and mode.
- Removes its rule, dedicated chain, and state during cleanup.
- Rolls back partially applied state on errors and handled signals.

This mechanism isolates the entire evaluator container's forwarded IPv4
egress. It does not alter the host clock or timing service configuration.

### 13.3 Expected behavioral sequence

```text
LIVE
  -> HOLDOVER
  -> DEGRADED while policy still permits consumption
  -> recovery through REJOIN or direct LIVE when within threshold
  -> LIVE
  -> HOLDOVER
  -> DEGRADED
  -> FAIL_CLOSED
  -> restore default policy and upstream access
  -> REJOIN or direct LIVE
  -> LIVE
```

The harness permits direct `LIVE` recovery only when the observed delta is
within the declared slew threshold. Otherwise, controlled `REJOIN` is
expected.

### 13.4 Required final markers

```text
HOST_TIMING_STATE_UNCHANGED=PASS
ORIGINAL_NODE_ENV_RESTORED=PASS
RESTORED_DEFAULT_LIVE=PASS
GAL2_SERVICE_STATE_RESTORED=PASS
GAL2_EXTERNAL_EVALUATION=PASS
```

## 14. Evaluation result classification and failure criteria

Report raw command output, relevant logs, contract records, and the generated
evidence directory for every failure or blocker.

### 14.1 Supply-chain failure

Any of the following is a failure:

- Published checksum mismatch
- Unexpected GPG primary fingerprint
- Invalid detached signature
- Tested-payload mismatch
- OCI blob, manifest, config, platform, or label mismatch
- Loaded Docker object outside the exact two-digest identity set
- Installed file hash, size, mode, or ownership mismatch

Do not continue execution after a supply-chain identity failure.

### 14.2 Environment compatibility blocker

Classify these as environment or compatibility blockers, not as successful
fault-injection results:

- Non-AMD64 host
- Podman or `podman-docker` substituted for Docker Engine
- Docker Engine below 28.3.3
- Inactive or unavailable `docker.service`
- Missing Docker bridge network
- Missing `iptables` command for the full harness
- Missing Docker-managed `DOCKER-USER` chain
- Docker native nftables configuration without the required chain
- Global IPv6 enabled on the evaluator container
- SELinux denial preventing the dedicated bind mounts or host Provider from
  reading SHM

Preserve the original environment and report the blocker before changing
firewall or SELinux policy.

### 14.3 Functional failure

Examples include:

- Node fails to reach consumption-ready `LIVE` with a valid credential and
  reachable API.
- `gal2-node doctor` fails after the documented readiness interval.
- SHM publication does not progress.
- The Provider cannot consume an authorized LIVE or safe DEGRADED contract.
- Recovery does not reach the state accepted by the harness within its
  built-in bounded deadline.

### 14.4 Safety failure

Any of the following is a safety failure:

- The Provider returns a GAL-2 timestamp in `FAIL_CLOSED`.
- The Provider does not raise `ProviderUnsafeError` after unsafe state has
  propagated within the harness timeout.
- A FAIL_CLOSED Provider result contains `gal2_epoch_ns`.
- A Provider value moves backward across the tested recovery boundary.
- Contract state permits consumption after the declared policy no longer
  authorizes it.
- The application silently falls back to raw host time after Provider refusal.

Record observed `uncertainty_ms` and compare it with the contract's declared
policy fields, validity, mode, reason, and `safe_to_consume` decision. Any
internal inconsistency should be reported. Evaluator-defined uncertainty
thresholds are welcome when they are labeled separately from the RC11
built-in acceptance criteria.

### 14.5 Host-boundary or cleanup failure

Any of the following is a failure:

- Host timing-service state changes across the evaluation.
- Original `/etc/gal2/node.env` bytes, owner, group, or mode are not restored.
- Managed firewall rules or state remain after cleanup.
- Default 6-hour and 72-hour policy values are not restored.
- Original GAL-2 service active and enabled state is not restored.
- Final default LIVE recovery fails while the API is available.

The supplied harness uses its own bounded transition deadlines. RC11 does not
declare a separate maximum `FAIL_CLOSED` overshoot. Tighter timing thresholds
are welcome when recorded as evaluator-defined tests with explicit acceptance
criteria, independently of the supplied harness result.

## 15. Uninstall and complete removal

### 15.1 Normal uninstall

```bash
sudo /usr/local/lib/gal2-node/uninstall.sh
```

Normal uninstall verifies ownership and identity, then removes the service,
container, control command, and ordinary installed support payload. It
deliberately preserves:

```text
/etc/gal2/node.env
/var/lib/gal2
/run/gal2
gal2-node:rc11-amd64-evaluator-c8515d4
/usr/local/lib/gal2-node/uninstall.sh
/usr/local/lib/gal2-node/FROZEN_TESTED_PAYLOAD.json
```

Because normal uninstall preserves `/etc/gal2/node.env`, it can preserve the
API credential. Use complete purge when credential removal is required.

### 15.2 Complete purge

```bash
sudo /usr/local/lib/gal2-node/uninstall.sh --purge
```

Complete purge verifies target ownership and exact image identity, then
removes the configuration, credential, persistent state, runtime SHM, exact
verified evaluator image, purge helper, and frozen identity anchor.

The uninstaller requires the Docker daemon so it can establish container and
image ownership. It refuses destructive removal if the expected installation
or image identity cannot be established.

The downloaded ZIP, extracted package, and evaluator-created evidence
directory remain outside the installed product paths. The evaluator may
archive or remove them separately after preserving required evidence.

The temporary API credential should also be revoked or disabled by GAL-2 after
the agreed evaluation window.

## 16. Bundled founder-operated evidence

The bundled external evidence records a PASS on:

```text
Ubuntu 24.04.4 LTS
Linux x86_64 under QEMU on Apple Silicon
Docker Engine 29.7.2
```

Observed modes:

```text
DEGRADED
FAIL_CLOSED
HOLDOVER
LIVE
REJOIN
```

Recorded validations include:

- Default LIVE Provider acceptance
- Safe DEGRADED Provider acceptance
- Recovery Provider acceptance
- Typed `ProviderUnsafeError` refusal in FAIL_CLOSED
- No Provider timestamp returned in FAIL_CLOSED
- Host timing state unchanged
- Original `node.env` restored byte-exact with root ownership and mode `0600`
- Original service state restored
- Managed firewall state removed
- Final default LIVE restoration

This evidence is founder-operated. It does not claim native AMD64 timing
characterization, real-duration AMD64 endurance, RHEL 10.2 compatibility,
independent validation, or production certification.

The bundled end-to-end run did not inject a new outage while a rejoin slew was
already active. The relevant RC11 behavior is covered by unit-level regression
and remains available for evaluator-defined additional testing.

## 17. Expected effort, credential window, and findings handling

The following planning estimates are non-contractual and begin only after a
compatible Linux AMD64 host with genuine Docker Engine is available:

| Activity | Planning estimate |
| --- | --- |
| Artifact verification, installation, configuration, and baseline checks | 30 to 60 minutes |
| Supplied accelerated external harness and immediate evidence review | 20 to 30 minutes |
| Evaluator-defined stress, endurance, or additional fault tests | At evaluator discretion |

The founder-operated accelerated harness completed in approximately 12
minutes on the declared QEMU environment. Native RHEL behavior, dependency
installation, security review, and investigation of blockers can materially
change the elapsed time.

The temporary credential's activation and expiration dates are communicated
privately in the corporate covering message, not committed to this public
document. Before execution, the evaluator should confirm that the credential
window is still active. GAL-2 will acknowledge, reproduce where possible, and
classify returned findings through the agreed correspondence channel. No
response-time SLA is asserted by this RC11 handoff.

## 18. Requested independent evaluator record

Please record and return:

- RHEL release and kernel
- Physical or virtual deployment and host CPU architecture
- Docker client and server versions
- Docker firewall backend if known
- firewalld state
- `iptables -nL DOCKER-USER` result
- SELinux mode and any AVC denials
- Installation result
- Loaded Docker object identity
- Container labels and network mode
- `gal2-node doctor` result
- Baseline Time Contract
- Python Provider demo result
- Host-side SHM readability and SELinux labels
- External harness result
- Complete generated evidence directory
- Cleanup and restoration result
- Any incompatibility, unexpected behavior, or documentation ambiguity
- Any evaluator-defined tests and their acceptance criteria, kept separate
  from the supplied RC11 harness criteria

Do not include the API credential in returned logs or evidence.

Send technical findings to `support@gal-2.com` or through the separately
agreed evaluator correspondence channel.

## 19. Product boundary

The GAL-2 API delivers GAL-2 Time created by the Protected Core.

The GAL-2 Node makes GAL-2 Time locally consumable.

The Time Contract governs whether an enrolled application may consume it.

Keep your timing stack. Install the GAL-2 Node. Connect the application once.
