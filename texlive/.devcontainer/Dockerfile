FROM debian:bookworm-slim AS installer
ARG REPO=https://mirror.ctan.org/systems/texlive/tlnet

WORKDIR /install-tl-unx
RUN apt-get update && apt-get install -y \
    fontconfig \
    perl \
    curl \
    xz-utils \
    make \
    && rm -rf /var/lib/apt/lists/*

COPY ./texlive.profile ./
RUN curl -L -o install-tl-unx.tar.gz ${REPO}/install-tl-unx.tar.gz \
    && tar -xzf ./install-tl-unx.tar.gz --strip-components=1 \
    && ./install-tl --profile=texlive.profile \
    && ln -sf /usr/local/texlive/*/bin/* /usr/local/bin/texlive
ENV PATH=/usr/local/bin/texlive:$PATH

RUN tlmgr update --self && tlmgr update --all \
    && tlmgr install latex latex-bin latexconfig latex-fonts \
    && tlmgr install amsmath \
        babel \
        carlisle \
        ec \
        geometry \
        graphics \
        hyperref \
        lm \
        marvosym \
        oberdiek \
        parskip \
        graphics-def \
        url \
    && tlmgr install latexmk chktex latexindent

RUN curl -L http://cpanmin.us | perl - App::cpanminus \
    && cpanm YAML::Tiny \
    && cpanm File::HomeDir

FROM debian:bookworm-slim
RUN apt-get update && apt-get install -y --no-install-recommends perl git wget \
    && rm -rf /var/lib/apt/lists/*

COPY --from=installer /usr/local/texlive /usr/local/texlive
COPY --from=installer /usr/local/share/perl /usr/local/share/perl

RUN ln -sf /usr/local/texlive/*/bin/* /usr/local/bin/texlive
ENV PATH=/usr/local/bin/texlive:$PATH
