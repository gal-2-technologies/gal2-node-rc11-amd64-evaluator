GAL-2 Node RC11 Linux AMD64 Evaluator

Private handoff for independent technical evaluation of GAL-2 Node v1.0.0-rc11 on Linux AMD64/x86_64.

This repository distributes one frozen evaluator package through GitHub Releases. It does not contain the GAL-2 Protected Core or the temporary API credential.

Supported environment

This evaluator package requires:

* Linux AMD64/x86_64
* systemd
* Docker Engine 28.3.3 or newer
* A running docker.service
* Docker bridge networking
* Python 3
* curl
* sha256sum
* tar
* unzip
* GnuPG
* Root or sudo access
* A temporary GAL-2 evaluator API key supplied through a separate private channel

Podman and podman-docker compatibility are not supported by this RC11 package.

For the complete fault-injection evaluation, the host must also provide:

* iptables
* Docker’s DOCKER-USER chain
* IPv4 Docker bridge forwarding
* No global IPv6 address on the evaluator container

Frozen release identity

Release:

v1.0.0-rc11-amd64-evaluator

Package source revision:

a22098a01462917ac4923ea9ffdb0a0f56e5c328

Final audit ZIP:

GAL2_NODE_LINUX_AMD64_RC11_EXTERNAL_AUDIT_FINAL_20260902.zip

ZIP SHA-256:

0db64868d6bfe1a7230a0e675fd24aec096e45a916e1813dcaac4d2d534efefb

Signed release archive:

GAL2_NODE_LINUX_AMD64_1.0.0-rc11-evaluator.tar.gz

Release archive SHA-256:

d452a276af1bf4bd2abb11dfa01f14f22f7c72f36eda9a3019c4269ef36ef553

Detached signature SHA-256:

5c56f92df04f37d73b13625f06aafe2d1faf2d5f199c3007982f73aef2a48773

Release public key SHA-256:

4342e36099b65889fc18fe06ef01063595ee15bd0d2d3df6cd3d81d4b28f1bfc

Expected release-signing fingerprint:

802C 8978 FF85 7550 60B6 D6BC 8AB8 59E4 D705 822F

External evidence archive SHA-256:

7777d141ef05245ff2cd76cba8eece9735b03474c134ecfb2f08bc5ee66d305f

Download

Download both release assets from:

https://github.com/gal-2-technologies/gal2-node-rc11-amd64-evaluator/releases/tag/v1.0.0-rc11-amd64-evaluator

Required downloads:

GAL2_NODE_LINUX_AMD64_RC11_EXTERNAL_AUDIT_FINAL_20260902.zip
GAL2_NODE_LINUX_AMD64_RC11_EXTERNAL_AUDIT_FINAL_20260902.zip.sha256

Do not use a release archive obtained from another location unless its SHA-256 matches the frozen identity above.

Verify the audit ZIP

From the download directory:

sha256sum --check GAL2_NODE_LINUX_AMD64_RC11_EXTERNAL_AUDIT_FINAL_20260902.zip.sha256

Expected result:

GAL2_NODE_LINUX_AMD64_RC11_EXTERNAL_AUDIT_FINAL_20260902.zip: OK

Extract it into a new working directory:

mkdir gal2-rc11-amd64-evaluation
unzip GAL2_NODE_LINUX_AMD64_RC11_EXTERNAL_AUDIT_FINAL_20260902.zip \
  -d gal2-rc11-amd64-evaluation
cd gal2-rc11-amd64-evaluation

Verify the signed package and bundled evidence:

sha256sum --check GAL2_NODE_LINUX_AMD64_1.0.0-rc11-evaluator.tar.gz.sha256
sha256sum --check GAL2_NODE_LINUX_AMD64_RC11_EXTERNAL_EVIDENCE_A22098A_20260902.tar.gz.sha256

Verify the release signature

Use an isolated temporary GPG home:

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

Confirm that GPG reports a good signature and that the primary fingerprint exactly matches:

802C 8978 FF85 7550 60B6 D6BC 8AB8 59E4 D705 822F

A good signature verifies the artifact against the supplied public key. The fingerprint should also be compared through an independent trusted GAL-2 channel.

Extract the package

tar -xzf GAL2_NODE_LINUX_AMD64_1.0.0-rc11-evaluator.tar.gz
cd GAL2_NODE_LINUX_AMD64_1.0.0-rc11-evaluator

Verify the frozen tested payload:

sha256sum --check SHA256SUMS.tested-payload.txt

All listed files must report OK.

Environment preflight

uname -m
systemctl is-active docker
systemctl is-enabled docker
docker version --format 'SERVER_VERSION={{.Server.Version}}'
docker info >/dev/null && echo DOCKER_DAEMON=PASS

Expected architecture:

x86_64

The Docker service and daemon must be active.

Review the evaluation plan

Before installing or modifying the host:

python3 evaluation/run_external_evaluation.py --plan-only

Expected final line:

GAL2_EXTERNAL_EVALUATION_PLAN=PASS

Install

From the extracted package directory:

sudo ./scripts/install.sh

The installer validates:

* Tested payload checksums
* Frozen runtime archive identity
* OCI graph and blob digests
* OCI labels
* Linux AMD64 platform
* Docker object identity
* Installed file ownership and modes

The installer enables the service but does not start it automatically.

Configure the temporary credential

Edit:

sudoedit /etc/gal2/node.env

Set the separately supplied evaluator credential:

GAL2_API_KEY=TEMPORARY_EVALUATOR_KEY

Do not place the API key in this repository, an issue, a log, an evidence archive, or a screenshot.

Verify configuration ownership and mode:

sudo stat -c 'OWNER=%U:%G MODE=%a PATH=%n' /etc/gal2/node.env

Expected:

OWNER=root:root MODE=600

Start and verify

sudo gal2-node start
sleep 40
sudo gal2-node doctor

A consumption-ready installation reports:

shm_publication=PASS
gal2_time_ready=PASS
DOCTOR=PASS

Inspect the local Time Contract:

gal2-node contract

The authoritative application decision is:

safe_to_consume

Applications must not infer authorization from mode alone.

Run the supplied Python Provider demo

PYTHONPATH=/usr/local/lib/gal2-node/sdk/python \
python3 /usr/local/lib/gal2-node/examples/python/gal2_demo.py

A successful enrolled-application read reports:

"status": "ok"
"safe_to_consume": true
"raw_host_time_fallback": false

On a SELinux-enforcing RHEL host, also confirm that this host-side Provider can read:

/run/gal2/contract-v1.shm

The Node uses Docker private bind-mount relabeling with :Z. Native RHEL 10.2 behavior is part of the requested independent compatibility evaluation.

Run the complete external evaluation

The full harness temporarily applies bounded evaluation policies and controlled Docker egress isolation. It must be run from the extracted package directory.

sudo python3 evaluation/run_external_evaluation.py \
  --execute \
  --evidence-dir /tmp/gal2-rc11-external-evidence \
  --release-archive ../GAL2_NODE_LINUX_AMD64_1.0.0-rc11-evaluator.tar.gz \
  --expected-release-sha256 d452a276af1bf4bd2abb11dfa01f14f22f7c72f36eda9a3019c4269ef36ef553 \
  --release-signature ../GAL2_NODE_LINUX_AMD64_1.0.0-rc11-evaluator.tar.gz.asc \
  --release-public-key ../GAL2_RELEASE_SIGNING_PUBLIC_KEY_RC11.asc

A successful run ends with:

HOST_TIMING_STATE_UNCHANGED=PASS
ORIGINAL_NODE_ENV_RESTORED=PASS
RESTORED_DEFAULT_LIVE=PASS
GAL2_SERVICE_STATE_RESTORED=PASS
GAL2_EXTERNAL_EVALUATION=PASS

The evaluation exercises:

LIVE
HOLDOVER
DEGRADED
FAIL_CLOSED
REJOIN
LIVE

It also verifies Provider acceptance while policy permits consumption and typed Provider refusal without returning a timestamp after FAIL_CLOSED.

Cleanup

Normal uninstall preserves configuration, persistent state, runtime SHM, the frozen image, and the verified purge helper:

sudo /usr/local/lib/gal2-node/uninstall.sh

Complete purge removes the preserved package state and the exact verified evaluator image:

sudo /usr/local/lib/gal2-node/uninstall.sh --purge

Declared evaluation scope

This is a controlled technical evaluation candidate, not a claim of general production readiness.

The founder-operated AMD64 package and external harness were validated under Linux AMD64 emulation on Apple Silicon. Those results do not claim native x86_64 execution characteristics or real-duration AMD64 endurance. Timing, uncertainty, latency, and slew magnitudes observed under emulation should not be treated as representative of native AMD64 hardware.

The bundled external run exercised LIVE, HOLDOVER, DEGRADED, FAIL_CLOSED, and REJOIN, but did not inject a new outage while a rejoin slew was already active. RC11 changed SHM record validation so an established active slew remains publishable across HOLDOVER, DEGRADED, and FAIL_CLOSED. That path is covered by unit-level regression but was not exercised end to end in the bundled run. It remains available for independent evaluator testing.

The frozen OCI runtime contains one timestamp-based gal2_ixoye_observer.cpython-313.pyc corresponding to its included source metadata. The complete OCI archive and all OCI blobs are frozen by cryptographic digest. Removal of generated bytecode and an explicit build-time rejection invariant are tracked as RC12 build-hygiene work.

Requested evaluator feedback

Please record:

* RHEL version and kernel
* Native architecture
* Docker client and server versions
* SELinux mode
* Installation result
* Docker image identity result
* gal2-node doctor result
* Python Provider demo result
* Host-side SHM readability
* External harness result
* Generated evidence directory
* Any incompatibility, unexpected behavior, or documentation ambiguity

Product boundary

GAL-2 does not replace or discipline the host clock, NTP, PTP, GNSS, chrony, or systemd-timesyncd.

The GAL-2 API delivers GAL-2 Time created by the Protected Core.

The GAL-2 Node makes GAL-2 Time locally consumable.

The Time Contract governs whether an enrolled application may consume it.

Keep your timing stack. Install the GAL-2 Node. Connect the application once.
