FROM debian:12-slim AS installer
ARG REPO=https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/tlnet

WORKDIR /install-tl-unx
RUN apt-get update && apt-get install -y \
    fontconfig \
    perl \
    wget \
    xz-utils \
    && rm -rf /var/lib/apt/lists/*
COPY ./texlive.profile ./latexindent ./
RUN chmod +x ./latexindent
RUN wget ${REPO}/install-tl-unx.tar.gz \
    && tar -xzf ./install-tl-unx.tar.gz --strip-components=1 \
    && ./install-tl -repository ${REPO} --profile=texlive.profile \
    && ln -sf /usr/local/texlive/*/bin/* /usr/local/bin/texlive
ENV PATH /usr/local/bin/texlive:$PATH
RUN tlmgr update --self && tlmgr update --all \
    && tlmgr install latexmk chktex \
    && wget -O latexindent https://github.com/cmhughes/latexindent.pl/releases/download/V3.23.6/latexindent-linux \
    && chmod +x latexindent

FROM debian:12-slim
RUN apt-get update && apt-get install -y --no-install-recommends \
    perl \
    wget \
    git \
    && rm -rf /var/lib/apt/lists/*
COPY --from=installer /usr/local/texlive /usr/local/texlive
RUN ln -sf /usr/local/texlive/*/bin/* /usr/local/bin/texlive
COPY --from=installer /install-tl-unx/latexindent /usr/local/bin/texlive
ENV PATH /usr/local/bin/texlive:$PATH
