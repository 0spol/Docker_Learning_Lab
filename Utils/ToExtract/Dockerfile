FROM ubuntu:latest

RUN apt-get update && \
    apt-get install -y \
    dpkg \
    rpm2cpio \
    cpio \
    tar \
    gzip && \
    apt-get clean

WORKDIR /files

CMD bash -c "for file in *; do \
    case \"\$file\" in \
        *.deb) mkdir \"\${file%.deb}\" && dpkg-deb -x \"\$file\" \"\${file%.deb}\" && rm \"\$file\" ;; \
        *.rpm) mkdir \"\${file%.rpm}\" && rpm2cpio \"\$file\" | cpio -idmv -D \"\${file%.rpm}\" && rm \"\$file\" ;; \
        *.tar.gz) mkdir -p \"\${file%.tar.gz}\" && echo \"Descomprimiendo: \$file\" && tar -xzf \"\$file\" -C \"\${file%.tar.gz}\" && rm \"\$file\" ;; \
        *.tar) mkdir -p \"\${file%.tar}\" && echo \"Descomprimiendo: \$file\" && tar -xf \"\$file\" -C \"\${file%.tar}\" && rm \"\$file\" ;; \
    esac; done"
