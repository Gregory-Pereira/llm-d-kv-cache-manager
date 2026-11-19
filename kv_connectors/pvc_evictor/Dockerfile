# Digest pinned to a build containing perl-base 5.40.1-6+deb13u1 (fixes CVE-2026-57432,
# Snyk SNYK-DEBIAN13-PERL-17960058). Bump the digest when refreshing the base image.
FROM python:3.12-slim@sha256:5e2bdb1b0d542d5039cffb8181c65930093949194a65c365dfb1b0fe13a4fa5b

WORKDIR /app

# llmd_fs_backend.event_publisher: required when STORAGE_EVENTS_ENDPOINT is set (#605 BlockRemoved).
# Crawler discovery is path-only (#611) and does not import FileMapper/vLLM.
# Build from kv_connectors/: podman build -f pvc_evictor/Dockerfile -t pvc-evictor:tag .
RUN python3 -m pip install --no-cache-dir msgpack pyzmq \
    && mkdir -p /app/llmd_fs_backend \
    && printf '%s\n' '# Minimal package init for pvc_evictor event publishing.' > /app/llmd_fs_backend/__init__.py
COPY llmd_fs_backend/llmd_fs_backend/event_publisher.py /app/llmd_fs_backend/event_publisher.py

COPY pvc_evictor/config.py pvc_evictor/evictor.py ./
COPY pvc_evictor/utils/ ./utils/
COPY pvc_evictor/processes/ ./processes/

ENV PYTHONPATH="/app:${PYTHONPATH}"

CMD ["python3", "evictor.py"]
