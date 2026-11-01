FROM debian:trixie-slim AS builder

RUN apt-get update && apt-get install -y curl ca-certificates

WORKDIR /download
RUN curl -L -o model.llamafile https://huggingface.co/Mozilla/TinyLlama-1.1B-Chat-v1.0-llamafile/resolve/main/TinyLlama-1.1B-Chat-v1.0.Q4_K_M.llamafile
RUN echo "29eca81f9f3a639029aa1f35cb1f28a9411aafbc54f12b920ca904177177a965  model.llamafile" | sha256sum -c -

FROM debian:trixie-slim

RUN useradd --system --no-create-home llamafile && \
    mkdir -p /opt/llamafile && \
    chown -R llamafile:llamafile /opt/llamafile

COPY --from=builder --chown=llamafile:llamafile /download/model.llamafile /opt/llamafile/model.llamafile
RUN chmod +x /opt/llamafile/model.llamafile

USER llamafile
WORKDIR /opt/llamafile
ENV HOME=/opt/llamafile

EXPOSE 8080

ENTRYPOINT ["/bin/sh", "/opt/llamafile/model.llamafile"]
