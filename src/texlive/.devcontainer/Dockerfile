FROM buildpack-deps:kinetic-scm AS builder
ARG CHKTEX_MIRROR="http://download.savannah.gnu.org/releases/chktex"
ARG CHKTEX_VERSION=1.7.8
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get install -y --no-install-recommends cpanminus make gcc g++ libc6-dev \
    && rm -rf /var/lib/apt/lists/*
RUN cpanm -n -q Log::Log4perl \
    XString \
    Log::Dispatch::File \
    YAML::Tiny \
    File::HomeDir \
    Unicode::GCString
WORKDIR /tmp/chktex-builder
RUN curl -fL -o- ${CHKTEX_MIRROR}/chktex-${CHKTEX_VERSION}.tar.gz | tar xz --strip-components 1 \
    && ./configure \
    && make

FROM texlive/texlive:latest-basic
COPY --from=builder /usr/local/lib/x86_64-linux-gnu/perl /usr/local/lib/x86_64-linux-gnu/perl
COPY --from=builder /tmp/chktex-builder/chktex /usr/bin/chktex
RUN tlmgr update --self --all \
    && tlmgr install latexmk latexindent texliveonfly \
    && tlmgr update --all \
    && texhash
RUN ln -s /usr/local/texlive/2022/bin/x86_64-linux/latexmk /usr/bin/latexmk \
    && ln -s /usr/local/texlive/2022/bin/x86_64-linux/latexindent /usr/bin/latexindent \
    && ln -s /usr/local/texlive/2022/bin/x86_64-linux/texliveonfly /usr/bin/texliveonfly
